# Async runtimes

Due to current young state of the async Rust ecosystem,
it is impossible for `heim` to be abstract over the needed async primitives
(filesystem operations, network operations and concurrency basics in our case),
therefore, it provides integration with the most popular async runtimes
in order to reduce code bloat and possible runtime overhead.

## Existing integrations

 * [`tokio`](https://tokio.rs): see ["tokio" section](./tokio.md)
 * [`async-std`](https://async.rs): see ["async-std" section](./async-std.md) 

## Another option

If you are using another async runtime
or want to use `heim` in a blocking manner,
you can use fallback option, refer to ["Other use cases" section](./polyfill.md)
for additional details.

## References

 * [https://www.reddit.com/r/rust/comments/f9gkn5/why_arent_async_runtimes_abstracted_using_a_trait/]()
 * [https://www.reddit.com/r/rust/comments/ej649l/a_summary_of_the_current_state_of_async_rust/]()
 * [https://www.reddit.com/r/rust/comments/dpjlkt/can_anyone_give_me_a_high_level_summary_of_the/]()
 * [https://www.reddit.com/r/rust/comments/d6pw43/will_crates_like_tokio_mio_and_futures_be_still/]()