# Object Oriented Programming Features of Rust

## Characteristics of Object-Oriented Languages

* Objects
  * Rust has structs, enums
* Encapsulation
  * Rust has `pub`
  * Structs marked as `pub` will only make the Struct itself public, not the fields
* Inheritance
  * Rust does not have inheritance
  * Can use traits for code deduplication, but polymorphism is a whole other matter
    * Rust does allow for _bounded parametic polymorphism_, using generics to impose constraints on what generic types must provide.

## Using Trait Objects That Allow for Values of Different Types

* GUI example- very similar to the word processor GUI example in Design Patterns
* Use `dyn` to pass dynamically dispatched traits around
  * `pub components: Vec<Box<dyn Draw>>,`
* Traits that are intended to be used as trait objects must be "object safe"
  * Cannot return own type
    * Own type may not be known at runtime, so won't know what to expect back from method
  * Cannot use generic type parameters
    * Own type may not be known at runtime, so won't know what the type parameters are
  * Otherwise, compile-time error
* `Self` is an alias for the type of the current struct passed in self arg

## Implementing an Object-Oriented Design Pattern

* Possible to define a method that is only valid when called on a `Box` holding the type- `(self: Box<Self>` 
* Using `Option` to wrap a property purely so that it can be modified- can't take ownership of a property in Rust if it would leave the struct without a valid value
  * Seems like a bit of a hack. But then, it's worth the cost of not having nulls.
* Uses `self.state.as_ref()`- is this not equivalent to `(&self.state)`?
