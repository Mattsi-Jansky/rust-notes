# Meeting 5 - Cargo and Collections

## Chapter 7 - Jocelyn

* Cargo lets you build and share crates
* What are binary artifacts used for?
  * Tools- eg build tools like `clippy`.
  * Similar to NPM you can include build tool dependencies and call them in your pipeline via `cargo` with `cargo clippy`
* Why can't you put multiple library crates?
  * Makes it simpler for Cargo developers
  * Linking gets complicated- if a package has two libraries then Cargo has to figure out which files to package into which crate and there may be duplicates across different crates
* Private fields in a struct can be accessed from their parent module
* Constructing modules from multiple files
  * using `pub mod name;` creates a child module and Cargo will look for a `name.rs` file to contain the implementation.
  * This way you don't have to use module brackets inside a file, you can define the module outside of the file and everything in that file is part of that module.
  * Can also do this with folders- add `mod.rs` to folder. This is a special filename (think of `index.js` in NodeJS) that Cargo will look to to find module definitions. From there you can declare children modules and Cargo will include those files/folders matching the module names as part of those modules.
* Jorge shared: http://www.sheshbabu.com/posts/rust-module-system/

## Chapter 8 - Collections

* Why can you not read a value you get from a vector after adding something else to the vector?
  * Think of it in terms of the compiler's perspective- we know that adding an element to the vector won't modify the value we already fetched from it, but the compiler doesn't. It only sees that we get a reference to an variable that belongs to a mutable object, and called a method that may mutate that object. So it thinks the variable we're referring could have been mutated.
