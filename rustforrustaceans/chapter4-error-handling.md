# Chapter 4, error handling

* When implementing own error type common good practices..
  * Implement `std:;error:Error` trait which provides common functions for introspecting errors
  * `Debug` and `Display`, so that callers can log your error
  * `Send` and `Sync` so that errors can be shared across boundaries
* Opaque errors
  * Being overly specific about error types makes your API more complex, more difficult for the consumer to handle errors. If it isn't necessary, just use one error type. Only use multiple types if the different errors are recoverable in different ways, i.e. it is meaningful for the consumer to treat them differently
* Expose your errors with `Box<dyn error-type>` so that they are smaller on the stack, have less performance impact on the happy path
* Because `Box` does not implement `Error`, to use a dynamic error pointer consider implementing your own `BoxedError` type that implmenets `Error` trait (and smart pointer wraps `Box`?)
* 
