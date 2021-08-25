# Advanced Features

## Unsafe Rust

* Static analysis is conservative: better to reject some valid programs than accept some invalid ones
  * Unsafe Rust is telling the compiler "Trust me, I know what I'm doing" 
  * If you use it incorrectly, problems including memory safety and null pointer dereferencing can occur
* Unsafe Rust also enables you to do things that just can't be done in safe Rust, like access low-level operating system APIs or writing your own OS
  * If Rust didn't let you make unsafe programs, you just couldn't do certain tasks
* Use `unsafe {... }` to enter an unsafe block
* Unsafe block lets you do five things you can't in regular Rust
  * Dereference a raw pointer
  * Call an unsafe function or method
  * Access or modify a mutable static variable
  * Implement an unsafe trait
  * Access fields of `union`s
* Unsafe block does not disable all safety checks
  * Borrow checker et al all still run. It only allows you to do those five things above that you can't do otherwise.
* Should always wrap unsafe code in safe abstractions, to prevent the unsafe Rust leaking to the rest of the app
* Dereferencing a Raw Pointer
  * Unsafe rust features types called _raw pointers_, that can be immutable or mutable and are written as `*const T` and `*mut T` respectively.
  * Raw pointers...
    * Are allowed to ignore the borrowing rules, having both immutable and mutable pointers or multiple mutable pointers
    * Aren't guaranteed to point to valid memory
    * Are allowed to be null
    * Don't implement any automatic cleanup
  * Creating a raw poiner: `let r1 = &num as *const i32;` or `let r2 = &mut num as *mut i32;`
  * _Creating_ a raw pointer does not actually require an unsafe block, but accessing or _dereferencing_ them does
* `extern` functions

## Advanced Traits

* Associated types- associate a type to a trait without using generics
  * With generics if you associate a type to a trait implementation you can implement that trait multiple times
  * This may not be desirable- eg `Counter` trait, you can only count a collection in one way
  * Instead you can specify a type as a property of the trait such as `type Item;` and refer to that type in function signiatures like `fn next(&mut self) -> Option<Self::Item>`
* Operator override
  * So glad this exists! And the example is using `Point`!
* Default type parameters- specify a default type for a generic, like `Add<Rhs=Self>`
* Can implement the same method name from multiple traits, to call the correct trait use fully qualified syntax: `TraitName::method(&struct);`
  * With associated (static) functions we needs to be even more explicit: `
* SuperTraits- specify that the type implementing this trait must implement another trait also, like so: `trait TraitName: package:otherTratName {...}` eg `trait OutlinePrint: fmt::Display` requires types implementing this trait to also implement `Display`

## Advanced Types

* Newtype- use `Milimeters` instead of `u32`. All well and good and I agree, but more general programming practice rather than Rust-specific.
  * Call this "newtypes" (Char!)
* Type aliases: `type Kilometers = i32;`
  * As well as newtyping, can use to simplify long duplicated type statements
* the empty type: `!` indicates that a function will never return: `fn bar -> ! {... }`
  * Represents anywhere where execution (within a scope or the whole application) ends, and doesn't return anything. eg `continue` evaluates to nothing and moves to the next iteration, indefinite `loop` evaluates to nothing. `loop` with a `break` does evaluate to something

## Advanced Functions and Closures

* Closure in function: `fn do_twice: f: fn(i32) -> i32, arg: i32) -> i32 {..}`.
  * `fn(i32) -> i32` defines a closure type for the param
* Returning closure from a function: `fn returns_closure() -> Box<dyn Fn(i32) -> i32> { ... }`

## Macros

* Macros are a form of metaprogramming
* Macros are expanded at compile-time so can do things like implementing a trait on a given struct
* Macros are more difficult to read, understand, maintain than functions
* Macros also don't hoist- you have to define or bring them into scope _before_ calling them
* Defining a macro- `macro_rules! returntype {...}`
* Use `#[macro_export]` annotation to indicate a macro should be available whenever the crate in which the macro is defined is brought into scope.
* Body of macro definition is a series of patterns that may or may not match the arguments passed into the macro. If one matches, that arm is executed. Otherwise, compile error.
  * These patterns are more like regex than anything super strict. Powerful, and probably a pain to write.
* Procedural macros work a bit differently to regular macros
  * Take some code as an input, produce some code as output
  * used for things like annotations that can modify the behaviour of the code
  * Define procedural macro: `#[some_attribute]\npub fn some_name(input: TokenStream) -> TokenStream {...}`
* **These look awkward to test**
* Attribute-like macros
  * Provide two `TokenStream`s instead of one: One for the attribute params, one for the body of the codeblock.
* Function-like macros
  * Define functions you can call as if they were functionis, like `println!`.
