# Rust for Rustaceans chapter 10 - Concurrency

* Risks of concurrency
  * Correctness (data races leading to undefined behaviour)
  * Performance
    * Linear scalability  / perfect scalability - doubling number of cores doubles performance, 1-to-1 ratio
    * Most of the time you won't achieve linear scalability. Instead you may get sublinear, and occasionaly even negative scaling
  * Mutual exclusion (mutex) - shares resources can result in seemingly concurrent tasks running serially
  * Shared Resource Exhaustion
    * Even if you achieve perfect concurrency the resources you need may not be perfectly scalable, eg TCP socket exhaustion
  * False Sharing - two resources sharing a lock/resource they have to await even though they don't need to
    * They may use separate parts of the resource
    * You can split up locks of composite data into more fine-grained locks but you must be careful because that can lead to deadlocks
* Concurrency Models
  * Shared Memory
    * Straightforward- threads share regions of memory between them, perhaps guarded by mutex
    * Best for threads that need to work on some shared state
  * Worker Pools
    * Threads (workers) work entirely independently, coordinated by a worker pool on self-contained tasks that return some result
    * Best when the action you're performing is the same in every thread but the data you're performing it on varies
  * Rather than one pool of workers has many separate queues of jobs feeding into different actors

## Lower-level Concurrency

* Standard library provides `std::sync::atomic` module
  * Provides types with names starting `Atomic`, representing CPU primitives for atomic operations
    `AtomicUsize`, `AtomicI32`, etc
* Memory Operations
  * At a low level, two operations
    * `load` reads bytes from memory into a CPU register
    * `store`writes bytes from a CPU register into memory
  * If variable size spans more than certain number of bytes (determined by CPU architecture?) then the compiler splits that write into multiple store instructions
  * Risk here is that between each store instructions there is a moment where another CPU could read that variable and get the wrong value
  * CPUs provide lots of other store/load instruction variants that work atomically, for certain sizes
* Memory Ordering
  * Microarchitecture is complex. Can't guarantee that a value written to memory, even if done so first according to "wall clock time", will be read when the value is read by another thread. Caches and compiler changes complicate it.
  * Mental model- tree where leaf node is CPU registry. Root is "main memory" (RAM?). Inbetween nodes are caches. To read start at leaf node and work way up until you reach a node that has that value. Write works way up to main memory.
* `Ordering` type
  * Mechanism to define a concurrent order of execution, to tell the compiler which constraints on concurrent execution are valid
  * `Ordering` is an enum with variants like...
    * `Relaxed` - Guarantees nothing except that it is atomic.
    * `Release`, `Acquire`, `AcqRel` (Acquire Release) - sets up relationships between variables, ensures certain rules are followed
      * Loads and stores cannot be moved forward past a store with `Ordering::Release`
      * Loads and stores cannot be moved back before a load with `Ordering::Acquire`
      * An `Acquire` load of a variable must see all stores that happened before a `Release` store that stored what the load loaded
        * What ????
      * Example of mutex- using `Release` to store a lock and `Acquire` to acquire the lock ensures that these stores/loads can't be moved by the compiler in a way that breaks the mutex
    * `SeqCst` (Sequentially Consistent)
      * Ensures that stores/loads happen in written order- easier to understand but slower
* Compare and Exchange
  * Avoid deadlocks by only atomically updating if value in register matches the last one you observed
  * Eg mutexes
* Fetch methods
  * Compare and exchange is costly, can cause waits. For methods that commute (i.e. order isn't important) we can use `fetch` methods like `fetch_add`, `fetch_sub` etc.
  * Completes read _and_ store in a single atomic step, but has no guarantees as to what the value it's modifying may be
* **Sane Concurrency**
  * Start simple
    * Start with simple concurrent code, measure, and experiment. Only keep changes that make a demonstrable improvement to performance.
  * Write Stress Tests
    * Don't just test the typical cases, test extreme edge cases and so forth too
  * Use Concurrency Testing Tools
    * Loom explores all possible cross-thread interactions to check that your tests hold true in every possible case
      * Can be overkill for large codebases, can be many many possibilities and may catch bugs that are extremely unlikely to ever actually occur
    * ThreadSanitizer runtime checking
      * Made by Google, otherwise known as TSan
      * Adds hooks to log all your memory access and looks for problems such as race conditions
      * 
