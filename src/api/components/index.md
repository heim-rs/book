# Components

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
heim = { version = "*", features = ["process", "cpu", "memory"] }
```

Alternatively, you can use `full` feature, which enables all components at once:

```toml
heim = { version = "*", features = ["full"] }
```

It is strongly discouraged to use `full` feature, unless you are really planning
to use everything in `heim`; prefer to enable separate features instead.
