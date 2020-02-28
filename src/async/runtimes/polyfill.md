# Other use cases

Unfortunately, it will be a huge maintenance burden to support
all existing async runtimes in `heim`,
so instead it provides the fallback option,
which executes all operations in a blocking manner on the current thread.

It reduces runtime footprint, but might be a potential bottleneck,
especially when used in the async programs.\

In order to enable it, you need to explicitly opt-in into
`runtime-polyfill` feature in your Cargo manifest:

```toml
futures = "0.3"
heim = { version = "..", features = ["runtime-polyfill"] }
```

You still need some executor to drive futures to completion,
[`futures::executor`](https://docs.rs/futures/0.3.4/futures/executor/index.html)
will be used for this example.

## Example

Let's rewrite the same `uname -a` program, but in a blocking manner now:

```rust,edition2018
use heim::{host, Result};

fn main() -> Result<()> {
    // Note that program execution blocks at this point
    // and will not be continued till `host::platform()` resolves
    let platform = futures::executor::block_on(host::platform())?;

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
