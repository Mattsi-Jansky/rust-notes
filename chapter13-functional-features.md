# Functional Language Features: Iterators and Closures

## Closures

* Closures can capture values from the scope in which they're defined (Isn't there a term for this?)
* Closure syntax uses pipse: `|var| -> ...` `|var| { ... }`
* Closures can infer the type of their param
  * Must be called in order to infer, closures that aren't called can't infer the param type
  * Can't be called multiple times with different types, compile error
* Generic closures
  * `struct Name<T> where T: Fn(u32) -> u32`
  * `Fn` is a trait
* Three different types of closures mapping to the three ways a function can take a parameter (ownership, borrowing mutably, borrowing immutably)
  * `FnOnce` takes ownership
  * `FnMut` borrows mutably
  * `Fn` borrows immutably
  * This seems quite complex and probably difficult to read?
  * Can also use the `move` keyword to take ownership of a param

## Processing a Series of Items with Iterators

* Lazy iterators, similar to LINQ in C#
* Syntax: `.iter()`
* Can be used with `for` keyword- `for val in iter { .. }`
* `Iterator` is a trait, with method `next` which optionally gets the next item
* Some iterator methods take ownership of the iterator- eg `sum`, consumes the iterator and returns the sum of values.
* Other ones produce other iterators- eg `map`
* Can create your own iterators by implementing `Iterator` trait
  * Only need to implement the `Next` method

## Performance

They're fast.
