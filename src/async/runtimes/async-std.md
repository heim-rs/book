# async-std

Same to the ["tokio" section](./tokio.md), `heim` can be integrated
with [`async-std`](https://tokio.rs) runtime.

It is a preferred way to use `heim` if you already using `async-std`
to execute asynchronous routines.

In order to enable this integration, you need to explicitly opt-in into
`runtime-async-std` feature in your Cargo manifest:

```toml
async-std = "*"
heim = { version = "*", features = ["host", "runtime-async-std"] }
```

Now, in case if `heim` would want to access network or filesystem,
it will use `async-std` routines for that, utilizing the same
runtime instance you are using for other parts of your program.

## Compatibility

Note that `heim` operates with `std` types where it is required.\
For example, [`heim::disk::Partition::mount_point`](https://docs.rs/heim/*/heim/disk/struct.Partition.html#method.mount_point) method
returns `std::path::Path`, not the `async_std::path::Path`.\
It is up to users to convert types where it is needed.

## Example

Here is a quick re-implementation of the unix `uname -a` command,
which uses `#[async_std::main]` macro to execute async routines:

```rust,edition2018
use heim::{host, Result};

#[async_std::main]
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

You can check [other examples](https://github.com/heim-rs/heim/tree/master/examples) also.\
Note that they are using `tokio` as a runtime executor, but you can easily replace it
with `async-std` using the example from above as a reference.