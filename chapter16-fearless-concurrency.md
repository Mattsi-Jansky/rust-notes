# Fearless Concurrency

* As a systems-level programming language Rust offers more varied methods to handle concurrency than higher-level languages
* _Fearless Concurrency_- the principle that concurrency bugs are caught at compile-time rather than runtime

## Using Threads to Run Code Simultaneously

* `thread::spawn(|| {..});` to start a new thread
* To sleep a thread: `thread::sleep(Duration:from_milis(1));`
* To wait for a thread to complete: save the `handle` result from `thread::spawn` and use `handle.join`
* If a new thread function needs to take ownership of a variable in the parent scope, use the `move` keyword from chapter 13- `spawn(move || {..})`

## Using Message Passing to Transfer Data Between Threads

* Messages are sent between threads via a _channel_- one thread has a _receiver_, the other a _transmitter_.
* They reference the Go documentation in this section! That's really cool
* Declaring a new channel returns a tuple- `let (tx, rx) = mpsc::channel()`
  * `mpsc` stands for multiple producer, single consumer
  * Multiple threads can send to a receiver- somewhat similar to a message bus?
* `tx.send(val)` to transmit something. Returns a Result<T, E> that must be handled.
* To receive- `rx.recv()`. Again returns `Result<T,E>`.
  * `recv` blocks. For a non-blocking alternative: `try_recv`
* Can treat receiver as iterator: `for received in rx`
  * This waits- only breaks if other end is hung up
* To get multiple transmitters: `tx.clone()`

## Demo

* Uma asks about numbers, has some slightly different ones
* Uma asks about Protractor
  * Need to make sure we orchestrate switching feature flag on/off- forgot about that
  * 
* Need to share the details of database analysis
