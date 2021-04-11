# Chapter 9- Error Handling

## Unrecoverable Errors with `panic!`

* `panic!` halts execution (cleanly), used for _unrecoverable errors_
* Includes a backtrace in the log, showing where the panic happened
* Rust uses panic to ensure security
  * In C reading beyond an array's index will enter undefined behaviour, and may read into the memory space of another variable. Rust instead uses `panic!`, which prevents exposing private variables and closes a whole classification of security vulnerabilities off.

## Recoverable Errors with Result

* Result is used for _Recoverable errors_
* Part of the preamble so doesn't need to be included
* If error is recoverable use `Result<T,E>` in method signiature return type and return `Ok(result)` if it's OK, otherwise `Err(error)` if it fails.
* Can match on result to get type or error out, as seen in chapter 2
* Specifying a particular error type for `E` can give more information to the error
* io::error type (one of the options for `E`)
  * Has different kinds- `ErrorKind`, eg `ErrorKind::NotFound`
  * Get the kind of an error with `error.kind()`
* Using exclusively `match` to handle errors atm but there are other ways using lambdas
  * `unwrap_or_else`- pass a callback function that will be called if an error is received
* `Result.unwrap` function- returns inner value or panics, akin to `Option::get` in Java
* `Result.expect` function- throw specific error in panic if inner value doesn't exist
* It is idiomatic in Rust to _propogate_ an error- pass it up to the consumer so that the consumer can decide what to do (seems like common sense)
* `?` operator
  * Match on result, assign value if it exists otherwise (early) return error result (has to be used in context of function with `Result` return type?)
  * Calls `from` method of `From` trait to convert errors to the correct type
  * Returns early, which I'm not a fan of
  * Simplifies impementation, removes boilerplate

## To `panic!` or Not To `panic!`

* If you call panic you are making a decision on behalf of the consumer that this situation is unrecoverable
* Returning `Result` gives the consumer more options
* Only use `panic!` in "rare situations".
* `panic!` represents a failure in tests, so using `.unwrap()` is perfectly fine in tests
* Also perfectly fine to use in situations where the code can't possibly error- eg ` "127.0.0.1".parse().unwrap()`
* Makes sense to call `panic!` if your component gets into an unrecoverable state, either because the consumer passed values you can't work with or an external component is returning an invalid state that you can't fix.
  * I wasn't sure about this argument, but they do expand on it in a compelling way
  * If the consumer violates your contract that isn't an error the consumer can recover from, because there is a bug in the program- the programmer needs to be alerted and find the bug.
  * However, many of these cases can be handled with type safety (as the book rightly points out)
* Custom types for validation
  * Shows example of using type system to validate values, in case of chapter 2 guessing game example where user had to input integer between 1-100:
```
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }

        Guess { value }
    }

    pub fn value(&self) -> i32 {
        self.value
    }
}
```
