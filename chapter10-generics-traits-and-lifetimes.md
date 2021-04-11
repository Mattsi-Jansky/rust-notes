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

* 
