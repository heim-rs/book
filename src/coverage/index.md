# Platforms support

## Tier 1

`heim` is still in a MVP phase and due to the lack of resources
right now it targets to support Rust [Tier 1](https://forge.rust-lang.org/release/platform-support.html#tier-1) platforms only:

| OS      | Architecture |
| ------- | ------------ |
| Linux   | i686         |
| Linux   | x86_64       |
| macOS   | i686         |
| macOS   | x86_64       |
| Windows | i686         |
| Windows | x86_64       |

In addition, most recent OS versions are targeted right now;
for example, there might be a chance that it will not work on Linux 2.6.18.
This should be considered as a bug and if you have compatibility issues,
feel free to [create an issue](https://github.com/heim-rs/heim/issues/new) about it.

## Tier 2

Following targets are explicitly tested in the CI.\
Same to Rust Tier 2 platforms, these can be thought of as "guaranteed to build",
but no further guarantees about correctness are provided.

| Target                           |
| -------------------------------- |
| `aarch64-unknown-linux-gnu`      |
| `aarch64-unknown-linux-musl`     |
| `armv7-unknown-linux-gnueabihf`  |
| `armv7-unknown-linux-musleabihf` |
| `arm-unknown-linux-gnueabihf`    |
| `arm-unknown-linux-musleabihf`   |

## Other platforms

Other platforms are not implemented yet and there is no ETA on them.
Contributions are welcomed!
