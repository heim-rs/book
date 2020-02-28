# Development

## Project structure

`heim` is split into multiple sub-crates, which are gathered together
in the `heim` facade crate.

Most of the crates has very obvious names, ex. `heim-cpu`, `heim-memory`
or `heim-process` — they are responsible for each separate `heim` component.

`heim-common` contains commonly used types, such as `heim::Result`, `heim::Error`,
prelude module for `heim` development and also has a bunch
of platform-specific FFI bindings, which are utilized by other `heim-*` crates.

`heim-runtime` is an abtraction for all supported [async runtimes](../async/runtimes.md).
Note that instead of the calling, for example, `tokio::fs::read_link()` function,
you need to use `heim_runtime::fs::read_link()` instead.\
Same goes for `pin!`/`pin_mut!`, `join!` and `try_join!` macros, all of them are exported by `heim-runtime`.

## Crate structure

Each crate responsible for the corresponding `heim` module has the same files structure.\
For example, let's write `heim-cpu` from scratch and provide information about CPU statistics.

```
$ ls /src

os/
sys/
lib.rs
stats.rs
```

Where:

 * `os/` contains OS-specific traits and implementations, [same as Rust does](https://doc.rust-lang.org/std/os/index.html)
 * `sys/` contains platform-specific implementations
 * `lib.rs` should expose all public types and functions
 * `stats.rs` contains the `CpuStats` struct and async function which returns `heim::Result<crate::CpuStats>`

## Public interface

```rust,edition2018
use heim_common::prelude::wrap;

use crate::sys;

/// System CPU stats.
pub struct CpuStats(sys::CpuStats);

wrap!(CpuStats, sys::CpuStats);
```

`wrap!` macro generates `AsRef`, `AsMut` and `From` implementations,
which are allowing working with the "inner" `sys::CpuStats` struct.

It is strictly important that struct should only has these methods,
which are available on all platforms supported,
as done in the following example.

```rust,edition2018
impl CpuStats {
    pub fn ctx_switches(&self) -> u64 {
        self.as_ref().ctx_switches()
    }

    pub fn interrupts(&self) -> u64 {
        self.as_ref().interrupts()
    }
}
```

Linux additionally provides the amount of "soft interrupts",
but we can't expose it here, because it would not be portable.\
Instead, we should create the `CpuStatsExt` trait at `os/linux/stats.rs`:

```rust,edition2018
pub trait CpuStatsExt {
    fn soft_interrupts(&self) -> u64;
}

#[cfg(target_os = "linux")]
impl CpuStatsExt for crate::CpuStats {
    fn soft_interrupts(&self) -> u64 {
        self.as_ref().soft_interrupts()   
    }
}
```

Trait itself should be publicly accessible, but `impl` block
for our `crate::CpuStats` should be gated and implemented only
for `target_os = "linux"`

## Platform implementations

Now we need to create platform-specific implementation
and we will start with the `sys/linux/mod.rs` module.

`sys/linux/mod.rs` should be compile-gated too with `#[cfg(target_os = "linux")]`,
because it can only be used for Linux systems.
Same thing applies to all other platform-specific implementations.

Implementation for our `src/stats.rs` goes into the `sys/linux/stats.rs` module.

In the case of Linux it will contain few fields, which will be populated later:

```rust,edition2018
pub struct CpuStats {
    ctx_switches: u64,
    interrupts: u64,
    soft_interrupts: u64,
}

impl FromStr for CpuStats {
    type Err = Error;

    fn from_str(s: &str) -> Result<CpuStats, Self::Err> {
        // ..
    }
}
```

Now, we need to provide the async interface.
In case of Linux we need to parse the `/proc/stat` file
and create the `sys::CpuStats` struct with data from it.

Our `sys/linux/stats.rs` should declare one function:

```rust,edition2018
use heim_common::Result;
use heim_runtime as rt;

pub async fn cpu_times() -> Result<CpuStats> {
    rt::fs::read_into("/proc/stat")
}
```

What will happen here: `/proc/stat` will be read asynchronously
with the help of `heim_runtime::fs` and then parsed with help of the `FromStr` implementation.

Now let's go back to our public `CpuStats` struct.

It should declare a similar function too, but its implementation will be much simpler:

```rust,edition2018
use heim_common::Result;

use crate::sys;

pub async fn stats() -> Result<CpuStats> {
    sys::stats().map(Into::into).await
}
```

Since that `wrap!` macro from the start generates `From<sys::CpuStats> for CpuStats` implementation,
all we need now is to call the platform-specific function and wrap the result into public struct.

Same thing applies to all structs and functions returning `Future`s and `Stream`s --
platform-specific implementations should be received from the `Future` or `Stream` and wrapped
into a public struct via `Into::into`.

## Additional

Please, stick to the [Rust API guidelines](https://rust-lang-nursery.github.io/api-guidelines/checklist.html)
where possible.
