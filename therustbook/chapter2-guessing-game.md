# Chapter 2 - Programming a Guessing Game

* Guessing game- computer generates a number, we guess, tells us if it's higher or lower, repeat until we get the answer.
* `use std::io` brings the io _library_ from the std _library_.
  * Presumably each of these groups/modules/whatever is referred to as a "library", each library can have children and it results in a tree/namespace of libraries?
* `let mut guess = String::new();`
  * `let` to declare a variable
  * Variables are immutable by default
  * `mut` marks a variable as mutable
  * `::` indicates that we are calling an _associated function_. This is the same as a static method.
  * _type inferrence_- Rust has a static type system but we didn't need to declare the type of `guess`.
* `io::stdin().read_line(&mut guess)`
  * `io::stdin` could be written as `std::io::stdin` if we hadn't imported `io`
  * `&mut guess` passes `guess` as a _mutable reference_. ie, if it gets modified in the method it's passed to the original instance is modified also. 
    * Wait, why would you ever want a non-mutable reference? Just for memory optimisation? 
      * Answer: https://stackoverflow.com/questions/58581124/why-does-rust-allow-an-immutable-reference-to-a-mutable-variable
      * After creating a mutable reference, _only_ that reference can be used to modify the point in memory. Only one mutable reference can exist at a time (may be exceptions we haven't come across yet)
    * why does `read_line` not just return a string?
      * answer: https://users.rust-lang.org/t/why-do-functions-like-read-line-need-to-use-a-mutable-output-variable/3423
      * One of the core tenents of Rust's design is _making the costs explicit and controllable_. This helps with optimisation. In this case it has two benefits:
        * Allows for reuse of allocations. The compiler may be capable of doing this for you though, that is unclear.
        * Facilitates handling of incomplete reads. If something goes wrong in the I/O, you will have an incomplete string rather than nothing.
* `.expect("Failed to read line");`
  * The method returns an optional. In Rust this is called a `Result`. A `Result` is an enum with two values, `Ok` and `Err`. There are also specific `Result` types for different libraries- in this case `io::Result` but other libraries will return different ones.
  * `Result` exposes `expect`. Similar to `Optional::get` in Java this will panic, with the message you provided. Otherwise it returns the return value that `Ok` is wrapping. In this case it returns the number of bytes that the user entered into standard input.
* `println!("You guessed: {}", guess);`
  * Interpolation in Rust uses `{}`
* Adding a crate
  * There are two types of crate (package): _binary_ and _library_ crates. When we run `crate build` or `crate run` we're creating a _binary_ crate. But if we add a dependency to the `rand` crate we're downloading a _library_ crate that includes source code rather than a compiled binary.
  * Cargo will install the latest minor version, under the assumption that the public API is still compatible (ie the author is following semantic versioning correctly).
  * Crates come from https://crates.io/
    * The most popular crate is `rand`, probably due to people following this tutorial
  * After first being installed crates will remain at the same version, specified in `cargo.lock`. To explicitly update them you can call `cargo update`.
* `match guess.cmp(&secret_number) {
Ordering::Less => println!("Too small!"),
Ordering::Greater => println!("Too big!"),
Ordering::Equal => println!("You win!"),
}`
  * `cmp` compares two variables and returns an `Ordering` enum that can be matched like the `Result` was.
  * Each of the `enum => value` entries in a `match` statement is called an _arm_, consisting of a _pattern_ and a _value_.
* `let guess: u32 = guess.trim().parse().expect("Please type a number!");`
  * Converts the string to an int
  * _Shadows_ the previous `guess`
  * `parse` belongs to `String` and can parse a variety of numerical types, so we need to tell the compiler which type we're using by specifying `guess: u32`.
    * Type inferrence: Because `guess` is compared to `secret_number` the compiler will infer that `secret_number` should be `u32` as well
* `loop`, `break` and `continue`
  * Entering a loop is incredible simple; just wrap `loop` around a code block.
  * Exit it with `break;`. Not sure I'm fond of that; this requires you to read the loop more carefully to figure out where it exits. I think I would find a `while` more inuitive. 
