# Chapter 6, testing

* How tests work in background- Rust generates a _test harness_ which is a main function calling each function with `#[test]` annotation (and handling other stuff)
* Mocking
  * No one best mocking library has emerged yet but `mockall` is closest, though still under active development
* Doctests comments- add `#` before a line to ensure it is still included in test run, but not included in documentation
  * Good for repeated examples, hide the lines that have been shown already
  * Good for imports
* Doctests support attributes determining how the test is run, added immediately after the first three backticks
  * eg `shoukld_panic` if it is intended to panic, or `no_run` if it should only be tested for compliation and not actually run. `compile_fail` for examples of what not to do
* Fuzzing
  * `libfuzzer` for Rust
  * To explore fuzzing further, start with cargo-fuzz
* Property testing
  * `proptest` crate for Rust
* Miri
  * Testing tool for checking race conditions an unsafe code by interpreting the intermediary language MIR
* Loom
  * Testing tool for multi-threaded/parallel apps, adjusting ordering of threads to test for race conditions
* Performance tests
  * `hdrhistogram`- what range of runtime covers 95% of samples observed?
  * `criterion` - all-in-one perf test package, produces graphical reports
  * `black_box` function- ensure that the thing you are testing is not being removed by compiler optimisations
    * Compiler may remove the code under test because it doesn't appear to be doing anything, as we are not doing anything with the result (only testing the side-effect, the performance)
  * 