# Chapter 4 - Ownership

## What is ownership?

* Rather than garbage collect at run-time, Rust uses a series of compile-time rules to ensure correct memory management.
* The stack and heap
  * Stack- every memory space is fixed size known beforehand, last in first out, organised and efficient.
  * Heap- starts with initial size but can change, assigned by memory allocator. This process is called _allocating_. Pushing values to the stack is not allocating, it is a trivial operation due to the stack structure.
  * When you call a function the parameters are placed on the stack, but some may be pointers to the heap. Accessing the stack data is far faster than the heap data.
* Ownership rules
  * Each value in Rust has a variable that's called its _owner_.
  * There can only be one owner at a time.
  * When the owner goes out of scope, the value will be dropped.
* Two types of string
  * "String literal" that is immutable, has fixed length, assigned on the stack.
  * `String` object that may have variable length, assigned on the heap.
  * Create a `String` from a string literal: `String::from("hello")`
  * Append to a `String` with `push_str`
* When a heap variable goes out of scope, it is deallocated
  * `drop` method on that variable is called automatically
* Moving variables
  * If you assign a pointer from one variable to another variable, the first variable is invalidated and can't be used.
  * In this sense rather than copied the data has been _moved_.
*  Cloning variables
  * Common method `.clone()` clones any heap object, without invalidating the owner
* Copy
  * Primitive values like integers aren't cloned they are copied, the original value doesn't go out of scope
  * Can mark your own types as copy types to be stored on the stack using the copy trait
* Functions and ownership
  * Pass a heap object into a function and it'll no longer be valid in your scope, because it has been cloned for the function.
  * The variable will be deallocated within the function
  * However, returning a value also transfers ownership. So you can pass a variable back to the function that called your function.

## References

* Use reference operator `&` to reference a variable- literally making a pointer to the variable's pointer, in effect borrowing it without taking ownership.
* Mutable reference operator `&mut`- borrow a variable but keep the ability to modify it
  * Can only have one immutable reference within a scope
  * Can't mix mutable and immutable references
    * "Users of an immutable reference don’t expect the values to suddenly change out from under them!"
  * However, "scope" here is more fine-grained than the typical definition of "within some braces". If a variable is used once within a function then it is said to be out of scope after that usage, so a new mutable reference could be created after that point within the same pair of curly braces.
* Dangling references
  * If you create a variable within a scope and return a _reference_ to it instead of the variable itself, then the variable would be cleared up after it leaves scope and the reference would be pointing at nothing- a _dangling reference_. To prevent this Rust will throw a compiler error.
* The rules of references
  * At any given tyime you can have _either_ one mutable reference _or_ any number of immutable references
  * References must always be valid

## The Slice Type

* A reference to a part of a string
* `let slice = &string[x..y];`
* Creates a pointer to the same block of memory as the original, but posibly at a differnt point and of a different length.
* Rust's range syntax doesn't require a number at the start if you want to start from zero, ie `[..2]` is legal. 
  * The opposite is true to, from 2 to end: `[2..]`
  * Similarly, `[..]` returns the whole string but as a slice.
* Borrow rules ensure that the original string can't be mutated until the slice is out of scope
* Slices are string literals (and visca versa; a string literal can be referred to as a slice)
* Recommends using `&str` (slice) for parameters rather than `String`
  * Because `String` can easily be cast with `[..]`, but the other way around requires copying.
* You can also use slices with arrays (and perhaps other collections?)