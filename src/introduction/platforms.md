# Platforms support

`heim` is still in a MVP phase and due to the lack of resources
right now it targets to support [Tier 1](https://forge.rust-lang.org/release/platform-support.html#tier-1) platforms only:

| OS      | Architecture |
| :------ | ------------ |
| Linux   | i686         |
| Linux   | x86_64       |
| macOS   | i686         |
| macOS   | x86_64       |
| Windows | i686         |
| Windows | x86_64       |

In addition, most recent OS versions are targeted right now;
for example, there might be a chance that it will not work on Linux 2.6.
This should be considered as a bug and if you have compatibility issues,
feel free to [create an issue](https://github.com/heim-rs/heim/issues/new) for that.

Other platforms are not implemented yet and there is no ETA on them.
Contributions are welcomed!