# Introduction

![](https://raw.githubusercontent.com/heim-rs/heim/master/.github/readme-logo.png)

`heim` is a cross-platform Rust crate for retrieving information
about system processes and various system details
(such as CPU, memory, disks, networks and sensors).

`heim` has few key goals which define its development and public interface:

 * Async-first.\
    Refer to ["Async" section](../async/index.md) for additional details.
 * Cross-platform.\
   Any code from `heim` should just work on all [supported platforms](./platforms.md).
   OS-specific things do exist, but API design forces users to pay attention to them.
 * Modular design.
 * Idiomatic and easy to use.
