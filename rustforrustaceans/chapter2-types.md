# Chapter 2- types

* Misaligned access
  * When the bits of a value overlap the standard memory access blocks the CPU assigns, so the CPU has to do multiple reads and stitch the values together
* Byte alignment
  * In order to avoid misaligned access values must be _byte aligned_- they must start at multiples of their byte alignment. ie something that takes up 2 bytes (eg a 16-bit number) must be 2-byte aligned, meaning it must start at an address that is a multiple of 16.
  * If the previous value did not end at a multiple of 16 padding is inserted to make it fit.
* _layout_
  * The in-memory representation of a type, i.e. will it be 4-byte-aligned or 1-byte-aligned etc
* `repr` attribute can change the layout of a type
  * eg `repr(C)` makes it use the C memory layout. This makes it easier to interoperate with C in some contexts, and can be useful for manual pointer arithmetic because it is a predictable, deterministic standard unlike Rust's memory layout
* C memory layout is very simple, one value after another with padding to meet the byte alignment. All values of a type have the same layout which is convenient, but padding makes it inefficient.
* Rust memory layout is much more efficient, orders them by the largest byte alignment first which usually ensures there is no padding.
* Rust can also use `repr(packed)` which removes padding altogether and accepts that there may be misaligned values. This can lead to performance problems and bugs, and even runtime crashes from operations that are only supported on aligned memory values. But it can be used for efficiency when you have a type that is used many, many times in memory to reduce their memory footprint. Could be useful for embedded devices with low memory?
* _cache line_
  * A block of memory in the CPU's cache (not the RAM). Sized depending on CPU architecture, ie 32-bit for 32-bit CPUs and 64-bit for 64-bit CPUs. Is the smallest amount of space that can be accessed at one time, only one core can access it at a time.
* `repr(align(n))` can be used to give a type a greater alignment than it technically requires. This can be useful in concurrency contexts with multiple elements in contiguous memory, where two CPUs (or rather CPU cores?) may try to access two separate values that happen to be stored in the same cache line.
* Wide pointers aka fat pointers
  * Points to a memory location but also has other information necessary to access that memory, eg in a slice stores how long the value is. Used for dynamic variables.
* Orphan rule- your library must own either the type _or_ the trait in order to create a trait implementation on a type.
* Exceptions/nuance about the orphan rule
  * Blanket implementations- `impl<T> MyTrait for T where T:` implements trait for any type, can only be defined in the library that owns the trait. Doing so is considered a breaking change.
  * Fundamental types- some types are marked with the `fundamental` attribute which marks them as such a fundamental type that it must be possible to implement traits against them even if it breaks the Orphan rule. This includes `&`, `&mut` and `Box`.
  * Covered implementations- `From<MyType> for Vec<i32>`. Orphan rule has an exception for when you're using a third-party trait with a generic type that comes from your library.
* Marker traits and marker types
  * Types that only provide semantic meaning, aiding the compiler. Don't have any functionality/behaviour.
* 
