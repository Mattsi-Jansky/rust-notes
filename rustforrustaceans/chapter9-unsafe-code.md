# Rust for Rustaceans 9 - Unsafe Code

* Unsafe is Rust's mechanism for taking advantage of invariants that the compiler cannot check
* Not a way to write unsafe code, only a way to write safe code that the compiler is not smart enough to realise is safe
  * `unsafe fn` indicates to consumer that they must read the documentation to ensure that function's invariants are accounted for by consumer
* What can you do in an unsafe block?
  * Dereference raw pointers
  * Call `unsafe fn`s
    * `mem::transmute`, changing types
* So, why use unsafe..?
* Juggling raw pointers
  * Most common reason is to use Rust's raw pointer types: `*const T` and `*mut T`
    * Analagous to `&T` and `&mut T`, but they have no lifetimes and are not subject to ownership or borrow rules
    * Referred to as _pointers_ or _raw pointers_
  * Can cast a reference (`&`) to a pointer (`*`) in safe code, but can only cast a pointer (`*`) to a reference (`&`) in unsafe code
  * This section is a bit brief, could use some examples
  * Some examples here: https://sodocumentation.net/rust/topic/7270/raw-pointers
* Pointers can be helpful because they don't have liftimes, can circumvent lifetime rules.
  * Instead, you assure that it is safe when you use unsafe to access the pointer's value (the only risky thing you can do with a pointer)
* Pointer arithmetic
  * `Hashbrown` hash implementation has some examples of pointer usage: https://github.com/rust-lang/hashbrown/blob/master/src/raw/mod.rs
* To pointer and back again
  * Useful for changing types of values which cannot be done without unsafe, eg getting slices
  * `std::slice_from_raw_parts` from a slice's pointer `as_ptr` and `[]::len`
* Playing Fast and Loose with Types
  * Lightning-fast zero-copy type changing by just changing the type of a pointer
  * As before, changing types is a safe act but accessing it is unsafe
* Foreign Function Interfaces
  * Other compiled code does not have Rust's assurances so invoking them is unsafe
* I'll Pass on Safety Checks
  * Cater to situations where peak performance is paramount, you may wish to use `unchecked` methods to avoid runtime slowness
  * In practice, usually not worthwhile
* 
