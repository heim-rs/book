# Public API

## Components

`heim` provides information about various system components,
and for the sake of clarity, they are split into the corresponding modules.\
For example, there is
[`heim::process`](https://docs.rs/heim/*/heim/process/index.html) module for system processes routines,
[`heim::cpu`](https://docs.rs/heim/*/heim/cpu/index.html) for CPU related stuff,
[`heim::memory`](https://docs.rs/heim/*/heim/memory/index.html) for memory and swap info,
you got an idea.\
See [crate documentation](https://docs.rs/heim/*/heim/index.html) for all available modules.

Note that all these modules are not included into `heim` by default,
and you need to explicitly enable them with Cargo features, ex.

```toml
heim = { version = "*", features = ["process", "cpu", "memory", "runtime-tokio"] }
```

Alternatively, you can use `full` feature, which enables all components at once:

```toml
heim = { version = "*", features = ["full", "runtime-tokio"] }
```

It is strongly discouraged to use `full` feature, unless you are really planning
to use everything in `heim`; prefer to enable separate features.

## Platform-specific information

By default `heim` exposes only that information, which is guaranteed to be available
on all supported platforms.

Let's check [`heim::cpu::CpuStats`](https://docs.rs/heim/*/heim/cpu/struct.CpuStats.html)
struct as an example. It has two public methods: `CpuStats::ctx_switches` and `CpuStats::interrupts`,
which are returning amount of the context switches and interrupts correspondingly:

```rust
use heim::{cpu, Result};

#[tokio::main]
async fn main() -> Result<()> {
    let stats = cpu::stats().await?;
    println!("Context switches: {}", stats.ctx_switches());
    println!("Interrupts: {}", stats.interrupts());

    Ok(())
}
```

But for Linux there is also amount of software interrupts available,
and on Windows we might also want to get the syscalls amount too.\
`heim` follows Rust solution for [providing OS-specific information](https://doc.rust-lang.org/std/os/index.html)
and exposes extension traits for some of the publicly available structs.

In order to show software interrupts and syscalls amount,
we need to bring these traits into the scope and call the corresponding methods:

```rust
use heim::{cpu, Result};

#[cfg(target_os = "linux")]
use heim::cpu::os::linux::CpuStatsExt;

#[cfg(target_os = "windows")]
use heim::cpu::os::windows::CpuStatsExt;

#[tokio::main]
async fn main() -> Result<()> {
    let stats = cpu::stats().await?;
    println!("Context switches: {}", stats.ctx_switches());
    println!("Interrupts: {}", stats.interrupts());

    #[cfg(target_os = "linux")]
    println!("Software interrupts: {}", stats.soft_interrupts());

    #[cfg(target_os = "windows")]
    println!("Syscalls: {}", stats.syscalls());

    Ok(())
}
```

With this approach it is very easy to write cross-platform code
and platform-specific calls are more noticeable.
