# Meeting 4 - Structs and Enum

## Structs - I'm presenting

* Jorge points out the value of having multiple `impl` blocks- import a library and add your own functionality to it, akin to C#'s extension methods. Or import libraries that extend other libraries

## Enums - Jorge presents

* Jorge explaining a way to think of enums and tuples
  * They're similar to algebraic data types in functional languages
  * Tuples are product types- AND
    * A tuple of this AND this
  * Enums are sum types- OR
    * An enum of this OR this
* Jorge found an interesting behaviours of enums-
  * If you assign a value like `let thing = Enum::Thing` and `Thing` takes a variable then `thing` becomes an enum that has no value, as if it hasn't been initialised.
  * If you try to use `thing` as an enum instance it throws an error, but you can do `thing(1)` passing the value in to get a regular enum instance.
* Jorge shows an example of using enums with structs- a `LineItem` that has `itemNumber` and `quantity`, and an `Order` enum that has `Made`, `Fulfilled`, `Paid`, each taking a `LineItem`
* Jorge recommends a book for understanding how to use these sorts of enums better: https://www.amazon.co.uk/Domain-Modeling-Made-Functional-Domain-Driven/dp/1680502549
* Using `_` in matches with enums can be a disadvantage.
  * If you don't have a `_` and add a new enum, the compiler will warn you to add that case to the match. If you have a `_`, it won't.
* Discussion turned toward Rust game development
  * OpenGL tutorial: https://nercury.github.io/rust/opengl/tutorial/2018/02/08/opengl-in-rust-from-scratch-00-setup.html
  * Rust keynote on gamedev: https://www.youtube.com/watch?v=aKLntZcp27M&list=WL&index=349
  * Jonothan Blow responding to previous video, criticising Rust for gamedev: https://www.youtube.com/watch?v=4t1K66dMhWk&list=WL&index=348