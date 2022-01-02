# Chapter 3 - Common Programming Concepts

## Variables and Mutability

* Rust has constants
  * `const` keyword
  * Can be declared in global scope
  * Must be type annotated
  * Must be constant expression, not the result of a function call or other value that could only be computed at runtime.
* Underscores can be used in numeric literals for readability- ie 100000 becomes 100_000

## Data Types

* Two subsets of datatype: scalar and compound
* Scalar values are primitives- ints, floats, bools, chars 
  * integers
    * `i` integers are signed, `u` are unsigned. 
    * `isize` and `usize` types are "architecture" length; determined by the device's microarchitecture. 64-bit in a 64-bit machine, 32-bit on a 32-bit machine.
    * "The primary situation in which you’d use isize or usize is when indexing some sort of collection"- so avoid for most usecases
    * Overflow- will panic if in debug mode, otherwise wraps.
  * Floats- use `f64`
  * Chars
    * Single quotes for char, double for strings
    * Supports unicode
  * 
* Compound types "group multiple values into one type"; classes?
  * Tuples
    * Write values in parentheses- `(500, 12, 3)`
    * Values don't have to have same type- `let tup: (i32, f64, u8) = (500, 6.4, 1)`
    * Can destructure a tuple: `let (x, y, z) = tup;`
    * Can access a tuple by index using a period: `x.0 + x.1`
  * Arrays
    * Fixed length
    * Declare inline: `a = [1, 2, 3, 4];`
    * Uses type inferrence to know length of array. Otherwise, can declare type in square brackets with datatype, followed by semicolon then length- eg `a: [i32; 5 

## Functions

* Snake case (nooo!)
* Must type annotate parameters
* Statements and expressions
  * Statements make up most code- declaring and calling functions, assigning values
  * Expressions represent a value
  * Expressions don't end with a semicolon
* Can use `return` keyword to return early, but convention is not to
* Declare return type of function with `->`, eg `fn myfunc() -> i32 { ... }`

## Comments

* `//`
* Have to do it each line, no `/*` stuff

## Control Flow

* No brackets needed for `if` statements
* `if` is an expression, can be used to return or assign values
  * can be used with a `let`: `let x = if y == 0 { 5 } else { 6 };
* Loops
  * Three types: `loop`, `while`, `for`.
    * Would you ever use `loop` over `while` or `for` in practice?
  * Loops are expressions, so can assign values
    * eg `let result = loop { counter += 1; if counter == 10 { break counter * 2; }};`
    * `break` can be passed an expression to return
  * `for` works like `foreach` in C#
    * `for element in a.iter()`
    * Has to get iterable object from collection
  * Loop with range
    * `for number in (1..4)`
