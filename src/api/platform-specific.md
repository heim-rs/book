# Platform-specific information

By default `heim` exposes only that information, which is guaranteed to be available
on all supported platforms.

Let's check [`heim::cpu::CpuStats`](https://docs.rs/heim/*/heim/cpu/struct.CpuStats.html)
struct as an example. It has two public methods: `CpuStats::ctx_switches` and `CpuStats::interrupts`,
which are returning amount of the context switches and interrupts correspondingly:

```rust,edition2018
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

```rust,edition2018
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
