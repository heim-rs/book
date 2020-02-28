# Introduction

[![heim logo](https://raw.githubusercontent.com/heim-rs/heim/master/.github/readme-logo.png)](https://github.com/heim-rs/heim)

[`heim`](https://github.com/heim-rs/heim) is a cross-platform Rust crate for retrieving information
about system processes and various system details
(such as CPU, memory, disks, networks and sensors).

`heim` has few key goals which define its development and public interface:

 * [Async-first](../async/index.md).
 * Cross-platform.\
   Any code from `heim` should just work on all [supported platforms](./platforms.md).
   OS-specific things do exist, but API design forces users to pay attention to them.
 * Modular design.
 * Idiomatic and easy to use.

## License

Licensed under either of [Apache License 2.0](https://github.com/heim-rs/heim/blob/master/LICENSE-APACHE)
or [MIT license](https://github.com/heim-rs/heim/blob/master/LICENSE-MIT) at your option.

## Donations

`heim` is an open-source project, developed in my spare time.\
If you appreciate my work and want to support me
or speed up the project development,
you can do it [here](https://svartalf.info/donate/)
or support this project at [Open Collective](https://opencollective.com/heim-rs).