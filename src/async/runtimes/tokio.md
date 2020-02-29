# Tokio

`heim` can utilize [`tokio`](https://tokio.rs) async runtime
in order to fetch system information.

It is a preferred way to use `heim` if you already using `tokio`
to execute asynchronous routines
(directly, via `actix-web`, `hyper` or any other `tokio`-based library).

In order to enable this integration, you need to explicitly opt-in into
`runtime-tokio` feature in your Cargo manifest:

```toml
tokio = { version = "*", features = ["full", "macros"] }
heim = { version = "*", features = ["host", "runtime-tokio"] }
```

Now, in case if `heim` would want to access network or filesystem,
it will use `tokio` routines for that, utilizing the same
runtime instance you are using for other parts of your program.

## Example

Here is a quick re-implementation of the unix `uname -a` command,
which uses `#[tokio::main]` macro to execute async routines:

```rust,edition2018
use heim::{host, Result};

#[tokio::main]
async fn main() -> Result<()> {
    let platform = host::platform().await?;

    println!(
        "{} {} {} {} {}",
        platform.system(),
        platform.release(),
        platform.hostname(),
        platform.version(),
        platform.architecture().as_str(),
    );

    Ok(())
}
```

You can check [other examples](https://github.com/heim-rs/heim/tree/master/examples) also.