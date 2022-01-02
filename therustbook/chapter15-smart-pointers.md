# Chapter 15: Smart Pointers

* A pointer is a variable that contains an address in memory
* _Smart pointers_ act like pointers but also have additional metadata and capabilities
  * Rust implements various smart pointers in the standard library
  * Typically, in Rust a smart pointer owns the data it refers to
  * `String` and `Vec<T>` are smart pointers- they own the data they reference and provide metadata like length
* Smart pointers are typically implemented using `Struct`s
  * They are distinguished from ordinary structs by the `Deref` and `Drop` traits
    * `Drop` allows you to add a hook method that gets called when the variable drops out of scope
    * `Deref` allows an instance of the smart pointer to "behave like a reference"? Unclear what this means, more detail in following chapters

## Using Box<T> to Point to Data on the Heap

* `Box` allows you to store data on the heap, rather than the stack. Stack stores a reference to heap data.
  * So it turns what would have been a primitive into a pointed value instead? This is boxing. Suddenly makes sense why it's called `Box`
* Use box when...
  * Type whose size can't be known at compile time, and you need to know the exact size at run-time
  * Large amount of data, want to transfer ownership but ensure the data won't be copied/cloned when you do
    * Frequently transferring a large amount of data on the stack impacts performance
  * You want to own a value, but only care that it's a type that implements a particular trait rather than a specific type
    * Trait object, coverd in chapter 17
* Using Box: `Box::new(5)`
* Size of  _recursive type_ can't be known at runtime
  * Anything that can store another value of the same type inside itself- ie a tree or list
  * Because that recursion could continue infinitely, Rust doesn't know how much space to allocate it. Therefore, it must be stored on the heap.
* Cons list example
  * Const list data structure comes from Lisp.
  * Like a tuple whose second value is always the same type as the tuple- so you construct a new container containing a value, and another container
  * Last container has a value of "Nil" (Canonical name within the context of the datastruct, not nil as in Rust)
* When allocating size for an enum, Rust takes the size of the biggest variant
* Attempting to naively implement cons list gives `recursive type `List` has infinite size` error.
  * Can't figure out the maximum size of the enum because it's recursive
* This can be fixed by using `Box`- turns it into a linked list instead of a recursive self-reference

## Deref Trait

* Deref allows a struct to use the _dereference operator_ (`*`, it returns the referenced value- think it also consumes the reference) 
* `Box<T>` can be used as a reference: ```fn main() {
    let x = 5;
    let y = Box::new(x);

    assert_eq!(5, x);
    assert_eq!(5, *y);
}```

## Running Code on Cleanup with the Drop Trait

* Allows you to close streams as an object is killed, similar to dispose in Java/Csharp
* > For example, when a Box<T> is dropped it will deallocate the space on the heap that the box points to.
  * Why? If you do nothing the inner variables in `Box<T>` will be disposed of once they are out of scope (ie when the `Box<T>` is dropped), so why does it need to do anything when it is dropped? Maybe they're using some unsafe code that works differently?
* call `std::mem::drop ` to destruct/drop a variable forcefully
  * This may partially answer the question above- presumably this is _how_ they do it but still not clear on _why_

## Rc<T>, the reference counted smart pointer

* There are times when an object can have multiple owners
  * Explains with graphs: Imagine multiple nodes pointing to the same value, all thinking that they own it. Shouldn't be cleaned up until all nodes are dropped.
  * I think what they mean is that _conceptually_ in programming in general an object can have multiple owners; not that it can happen in Rust. `Rc<T>` must be a smart pointer that enables you to do that while still maintaining Rust's ownership system, such that `Rc<T>` is the owner of the inner value.
* Only for use single-threaded
* > We use the Rc<T> type when we want to allocate some data on the heap for multiple parts of our program to read and we can’t determine at compile time which part will finish using the data last.
* Create the new instance of `Rc::new`, then clone it as many times as necessary with `Rc::clone`


## RefCell<T> and the Interior Mutability Pattern

* Interior mutability pattern- objects acts as immutable reference to the outside consumer, but internally uses `unsafe` code to mutate itself.
  * `unsafe` is used when because we can guarantee that the borrow rules will be followed in run-time even when the compiler can't
* `RefCell<T>` will observe borrow rules during run-time instead of compile time- if you use it in the wrong way you'll get run-time errors instead of compile errors
* `RefCell<T>` is not threadsafe, similar to `Rc<T>`
* "you can mutate the value inside the RefCell<T> even when the RefCell<T> is immutable." - this seems weird to me. "Immutable" isn't the right phrase here, if you can mutate it then it isn't immutable.
* Use `borrow_mut` on ` RefCell<T>` to get a mutable ref to the inner value
* Example of usecase: Mocking a struct that is used as a reference
  * You want to record the behaviours used against the struct, while enabling the consumer to treat it as if it were a regular immutable reference
* `RefCell<T>` returns `Ref` and `MutRef` types that act as references/mutable references, implementing `Deref`
* If you violate borrowing rules with a `RefCell<T>`, you get a `panic!` in run-time
* You can also get multiple mutable references by making a `Rc<RefCell<T>>`- that way you can clone the smart pointer and get multiple smart pointers to the same area of memory
  * The book didn't seem to highlight how dangerous and how much of a bad idea this is

## Reference Cycles Can Leak Memory

* Rust doesn't guarantee safety against memory leaks, though they can only occur if you are doing something unusual.
* One unusual thing that can cause memory leaks: `RefCell<Rc<T>>` can leak if you have two that refer to one another. Then their reference counts will never reach 0 (so will never be dropped) because they always have a reference.
* To avoid creating cyclic references use `Rc::downgrade` instead of `Rc::clone`, returning a `WEAK<T>` REFERENCE TYPE
  * Weak references don't add to the count when checking for usages before droppign the value
  * Weak references don't express an ownership relationship
  * To do anything with a `Weak<T>` instance you must call `upgrade` which will return an `Option` with the `Rc<T>` value if it exists
* Eg to implement a tree this way each node would own their children and each child would have a `RefCell<Weak<T>>` reference to their parent.
* 
