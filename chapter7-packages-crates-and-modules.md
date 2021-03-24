# Chapter 6- Growing Projects with packages, Crates, and Modules

* _module system_ - series of tools that allow you to organize your code, namely:
  * Packages- lets you create, build, test crates
  * Crates- Tree of modules that produces a library or executable
  * Modules - Enableas controling the scope/privacy of "paths"
  * Paths: A way of naming an item (Namespaces?)

## Packages and Crates

* A crate is an artifact- either a binary (executable), or library
* The _crate root_ is a source file the compier starts from
* A _package_ is one or more crates providing some set of functionality
  * Package is defined by a _cargo.toml_ file
* A package cannot include more than one library crate
* If a package contains `src/lib.rs` then Cargo knows it has a library
* If a package contains `src/main.rs` then Cargo knows it has a binary
* You can put multiple source files in `src/bin/` to get Cargo to generate multiple binary crates for your package
* It is possible to have both a library and one or more binary crates in your package


## Modules

* Paths = namespaces, modules contain code and it can be public/private to expose it from the module or not.
* Modules can have sub-modules, even within one file
* `mod` keyword to define a module

## Paths

* Paths can be absolute or relative
  * Relative paths can use `self` or `super` to refer to current module or parent module. They can also use the names of the current of parent modules.
  * Absolute paths should start with eith `crate` to refer to current crate, or the name of another crate dependency.
* Rust convention is to use absolute paths
* All items are private by default
* Items in a parent module can't use private items in child modules, but items in child modules can use private items in their ancestor modules.
* Items can always access siblings- other items defined in the same module
* Modules, functions, enums and structs are all affected by pub/priv
* It's very clear from the way this chapter is written that the Rust team really thought hard about the namespaces problem and solved it thoroughly

## Structs and enums

* Some gotchas surround making these pub
* `pub` makes a struct available to be declared, but the fields are still private
* Can have some struct fields public, others private
* Public structs that have a private field _cannot be constructed using struct instantiation_, so _have to have a public constructor_ to be useful.
* If an enum is made public, all its variants are public. Much simpler

## Use

* It is idiomatic to only include up to the parent module of a function, not the function itself
  * This way when you call the function it is clear to the reader that you are calling something from another module and where it comes from
* However, it is idiomatic to use the full path when referring to structs, enums and other non-function items
* Can alias types with `as`: `use std::io::Result as IoResult;`
* Re-exporting- bring something into scope but also make it public such that other items importing this module get access to that module
  * `pub use crate::front_of_house::hosting`
  * Useful for forming a simple interface/signiature to the consumer while also separating out internal logic into small modules
* Nested paths
  * `use std::{cmp::Ordering, io}` imports both `std::cmp::Ordering` and `std::io`
  * Saves vertical space
* Glob Operator, `::*`
  * Brings all public items defined in a path to scole- `use std::collections::*;`
  * Don't use it too much, considered messy.

## Separating Modules into Files

* Can separate one module into separate files
* Put `mod modulename;` at the top of multiple files to indicate they're all part of the same module `modulename`
