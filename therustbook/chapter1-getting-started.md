# Chapter 1

## How to use this book (actually from chapter 0)

* > Chapter 18 is a reference on patterns and pattern matching...
  * Seems funny, I would expect pattern matching to be much earlier. This way concurrency is explained before pattern matching.

## Installation

* rustup-init
  * use `-y` to automate
  * Dotfiles: https://github.com/Mattsi-Jansky/dotfiles/commit/4881d703698d99fb90ed3ee1c04b692a4a9fb156

## Hello, World!

* `*.rs` extension
* Compile with `rustc`
  * Feels funny to be working in a compiled language like this again. Even in Csharp and Java you don't tend to think in terms of binaries because you have tools like Gradle and the dotnet CLI that make it streamlined as if it were interpreted.
    * I later found the `cargo run` command!
* > It’s good style to place the opening curly bracket on the same line as the function declaration, adding one space in between.
  * Ew
* `!` denotes a macro, as opposed to a function
* They are going out of their way to support Windows, adding exceptions to otherwise consistent explanations.

## Hello, Cargo!

* `cargo new <name>` inits a new project
  * New project structure is very minimal and simple
* Puts all code in `src/`
* Has a `Cargo.toml` and `Cargo.lock`, like `package.json` and `package.lock` in NPM/Yarn.
  * Happy to see that they use TOML
* `cargo build` & `cargo run` to build/run
* `cargo check` will check if your code compiles, faster than building it and said to be used by many Rustaceans
  * Seems odd to me- I'd rather periodically run tests rather than just check that it compiles
* 
