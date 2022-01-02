# Chapter 10 - Generic Types, Traits, and Lifetimes

## Generic Data Types

* Rust has generics, mostly works as you'd expect coming from languages like Java, C#
* One interesting feature, can pass collections (or rather slices) of generics like so: `list: &[T]`
* Generics can have traits that tell the compiler which ways they can be used, eg `std::cmp::PartialOrd` trait specifies that a type can be ordered (ie implements `>` and `<` operators)
* `impl` blocks of structs with generics can work in two different ways
  * Implement a method specifically for the struct with a specific type, eg `impl Point<f32>`
  * Implement a method for the struct generic, eg `impl<T> Point<T>`. In this case `<T>` features twice because the `impl` block needs to know that it's using a generic, but `Point` also needs to specify that it's targeting the generic implementation.
* Worth noting that by consuming itself methods on a struct with generics can create a new instance that changes the generic type, eg `impl<T, U> Point<T, U> ... fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W>`
* Generics don't impact performance - Rust uses "monomorphization" to compile the generics into specifics (ie creates a separate class for every possible type that may be used)

## Traits: Defining Shared Behaviour

* Traits are similar but not quite the same as interfaces in Java, C#, et al
* Declare a trait as follows
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```
* Implement a trait with `impl <struct> for <trait>` like so:
```rust
impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
```
* Can implement traits from other libraries but they need to be public (`pub trait`) and you have to import them
* Either the type or the trait must be locally defined. Can't implement an external trait on an external type.
  * Otherwise two different libraries could implement the same trait for the same type and Rust wouldn't know hich one to use, aka the Orphan Rule
* Traits can have default implementations. Just use curly braces and an implementation instead of a semicolon.
  * Default implementations can call other methods in the same trait, even if they don't have a default implmenetation

## Trait parameters

* Can specify that a parameter must implement a trait with `&impl <Trait>`, eg: `pub fn notify(item: &impl Summary) {...}`
* Can achieve the same thing using another syntax, _trait bound_ syntax, which is more similar to Java/C# generics like so: `pub fn notify<T: Summary>(item: &T) {`
  * This is more verbose but it can be clearer in cases where you have multiple parameters using the same trait, eg: `pub fn notify<T: Summary>(item1: &T, item2: &T) {...}`
  * However, this syntax also specifies that the type of `item1` and `item2` _must_ be the same concrete type, which is not true of `&impl`
* Can specify multiple traits for a single parameter using the `+` operator, eg `pub fn notify(item: &(impl Summary + Display)) {...}` or `pub fn notify<T: Summary + Display>(item: &T) {...}`
* Using multiple trait bounds can get long and awkward to read, like so: `fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {...}` to prevent that there is another syntax using a `where` clause, like so:
```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```
* You can also _return_ a type that implements a trait using `impl`, like so: `fn returns_summarizable() -> impl Summary {...}`
  * However, this won't work if your function and actually return multiple concrete types. This is probably due to monomorphization.
* 

## Validating References with Lifetimes

* Every reference in Rust has a _lifetime_
  * A liftime is a scope for which that reference is valid
* Like types lifetimes are inferred where possible, but sometimes must be explicitly stated
* Returning parameters from methods
  * If you have a function that may return one or another parameter passed to it (eg `longest(int,int)`) that makes it difficult for the compiler to infer the lifetime of those values, because either one could be returned.
  * You need to add generic lifetime parameters that define the relationship between the references: `fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {`
    * A lifetime is a generic?
* Lifetimes are generic- just as functions can accept any type when using generics, functions can accept references with any lifetime by specifying a generic lifetime parameter.
  * The important thing you're establishing is the _relationship_ between the references.
* Specify lifetime _annotation_ like so:
  * `&'a i32`, `&'a mut i32`
* You aren't specifying how long a lifetime will last- only that there must be some lifetime here, and the Rust will substitute that for the shortest lifetime of those available.
* Structs can hold references and must supply a lifetime for every reference
* Lifetime Elision
  * There exist _Lifetime Elision Rules_ that match particular deterministic situations where the lifetimes of a reference can be predicted
  * In these cases you don't need to specify a lifetime
  * How do you recognise these cases?
* Named _Input lifetimes_ and _output lifetimes_ depending on whether they're coming in via a param or out via return value
* 
