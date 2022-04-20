# Rust for Rustaceans chapter 8, Asynchronous Programming

* Poll type
  * enum of `Ready(T)` or `Pending`
  * "here you are or come back later"- the non-blocking operation type
  * Methods using `Poll` should start with `poll`
* `Future`
  * Trait that standardises use of `Poll`
  * Types that implement this trait know as _futures_
  * Represent something that may not be available yet
  * Equivalent of promise in other languages
  * Never poll a future after it has returned `Ready`. It may panic.
* `async`/`await` similar to csharp
  * `async fn`... `thing.method().await` (await comes afterwards not before, opposite syntax to csharp)
* Generators...
