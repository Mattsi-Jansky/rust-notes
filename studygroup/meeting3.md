# Meeting 3

## Liam presentation

* Memory cycles?
  * Using more memory makes reading from memory slower
* PJ makes a point about heap memory
  * Processor has to allocate and deallocate memory, significant overhead
  * Referred to as allocating
  * Liam refers to the waiter analogy from the book
* Ivan asks about the "capacity" information in a variable, from the chapter's diagrams
  * 
* Liam showed a very intersting chess example in which calling a method on a struct can drop that struct, because it borrows `self` and then `self` leaves the method's scope
* Cyryl's phrase
  * You can have shared state, you can have mutable state, but you can't have shared mutable state.
* Liam mentioned an interesting podcast featuring some prominent Rust people- On the Metal
