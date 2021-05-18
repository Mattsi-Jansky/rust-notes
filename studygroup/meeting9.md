# Study Group Meet 9

## Ivan presenting closures

* Javier asked about how many params a function can take in and return
  * Can take arbitrary number of params in, but can only return one value- value can be a struct or tuple to return multiple 
* Liam asked if Rust has an equivalent of `yield return` from csharp
  * It's an in-progress feature: https://github.com/rust-lang/rust/issues/43122
  * But because of how iterator traits work, you could implement it yourself
* The compiler infers the trait of the closure based on usages
* We're confused by the "All closures implement FNOnce..." line
  * If all closures were `FnOnce`, then there would be no point in having `Fn` because everything is borrowed mutably. Not really sure what this sentence means
  * https://stackoverflow.com/questions/65184768/how-can-it-be-that-all-closures-implement-fnonce?noredirect=1&lq=1
    * This explains that `FnOnce` is the base trait extended by the others- as shown in the docs "`Fn` and `FnMut` are subtraits of `FnOnce`

## Jocelyn presenting iterators

* `.zip` merges both values into tuple- the result is the same length as either input, with both values of each index
* Iterators are a zero-cost abstraction
* 
