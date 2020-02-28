# Async first

As it was mentioned before, `heim` is created with "async-first" idea in mind.

If you are not familiar with async programming in Rust,
refer to "[Asynchronous Programming in Rust](https://rust-lang.github.io/async-book/)" book
to learn about wider view on the topic.

## Motivation

It might be seen unusual for such a library to be so persistent on the
"async-first" approach.
The reason for that is because even the modern operating systems
might spend a lot of time gathering requested system information.

Most common argument against it is the
"how much time will one `syscall` take, am I right?",
but in a worse case scenarios, it might take some unknown and unpredictable
amount of time to do one simple operation, which is not what you
usually expect from your operating system.

For example, it might take seconds [to read one file](https://unix.stackexchange.com/questions/451673/how-can-cat-proc-pid-cmdline-take-several-seconds)
from the Linux `procfs` virtual filesystem; or if you accidentally
called `access(2)` on a NFS mount, performance slaps you in the face,
because now OS needs to make a network request to do that.

Acknowledging this fact, you might consider `async` keyword in the
`heim` public API as a mere marker of uncertainty on how much time
this operation will take; it really depends on what information
you are requesting and what operating system is used.

Thanks to the modern Rust async runtimes, it is very easy and safe
to do something else while waiting on this one operation,
so why should not we embrace it already?
