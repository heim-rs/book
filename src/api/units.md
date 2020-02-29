# Measurement units

`heim` heavily relies on the [`uom`](https://crates.io/crates/uom) crate
in order to expose proper measurement units where it is applicable.

In a short, that means that, for example, [`heim::cpu::CpuFrequency`](https://docs.rs/heim/*/heim/cpu/struct.CpuFrequency.html)
struct methods are returning `Frequency` type,
instead of the primitive types such as `u64`,
which are [prone](https://en.wikipedia.org/wiki/Mars_Climate_Orbiter#Cause_of_failure) to the logical bugs.

`heim` re-exports all used measurement quantities and units at [`heim::units`](https://docs.rs/heim/0.0.10/heim/units/index.html) module.\
Refer to [`uom`](https://docs.rs/uom/0.27.0/uom/#usage) documentation
on how to work with these types.

## Example

```rust,edition2018
use heim::{cpu, units, Result};

#[tokio::main]
async fn main() -> Result<()> {
    let freq = cpu::frequency().await?;

    println!("Current CPU frequency: {} GHz", freq.current().get::<units::frequency::gigahertz>());

    Ok(())
}
```
