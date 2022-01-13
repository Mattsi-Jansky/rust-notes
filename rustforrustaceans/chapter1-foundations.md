# Chapter 1- Foundations

* Memory
  * Not all memory is the same- difference between stack, heap, registers, text segments, mapped files, etc
  * "regions of memory"?
* Memory terminology
  * Value
    * The combination of a type and an element of that type's domain of values
    * Distinct from the _representation_ of that value in memory
  * Place
    * Value is stored in a _place_ which could be heap, stack, or elsewhere
  * Variable
    * A named value slot on the stack
  * Pointer
    * A value that holds the address of a region of memory (ie, points to a place)
    * References (`&x`) are considered pointers. On the stack they store the address of a space in memory.
      * Presumably only the compiler is aware of the read-only/mutable nature of references, different pointers do not differentiate on mutability at runtime
* Models of computer memory
  * High-level model
    * Helpful when thinking about code at the level of lifetimes and borrows
    * _flow_ - a dependency line traced from when a variable is first created to each time it is used (lifetime?)
    * Interesting example showing how borrow checker uses flows, similar to examples from The Rust Book
    * The borrow checker _checks that no incompatible flows exist concurrently_- i.e., there are no contradictions
      *  Eg having a flow that borrows a variable but no ownership flow for that variable would be a contradiction, can't borrow something that doesn't have an owner
    * Two variables with the  same name are distinct flows- _shadowing_ dictates that the later variable effectively replaces the first one, with a new flow.
  * Low-level model
    * Good for doing unsafe code, dealing with raw pointers
    * Variable points to a space in memory- a _value slot_. The size of the slot is determined by the type- ie `let x: usize` registers a value slot big enough for a `usize`.
    * `&x` does not change when you modify `x`- it's value is a pointer.
* Memory Regions
  * There are various different regions of memory. The most important to us are the stack, heap and static memory.
  * I think what they refer to as memory regions is one layer of abstraction above where the values are actually stored
  * Stack
    * Usual explanation of the stack- a stack of frames, first frame is your `main` function, each time a function is called add a stack frame. After a function returns, the stack frame may be reclaimed.
    * **Stack frames are very closely tied to the notion of lifetimes in Rust**. Especially the fact they they eventually disappear after the function returns.
  * Heap
    * 
