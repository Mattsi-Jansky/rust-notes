# Chapter 18 Patterns and Matching

## All the Places Patterns Can Be Used

* Match arms
  * An "arm" is one part of a match expression, ie `PATTERN => EXPRESSION,`.
  * Matches must be exhaustive
* `if let` expressions
  * Can be used in long if-else trees
  * Some of these expressions can be confusing, like `if let Ok(age) = age {`
    * I _think_ what is happening here is that both `age`s are actually separate variables altogether. Age is being assigned to a deonstructed variable that is exposing a new variable of the same name.
    * Next section confirms my understanding is correct, provides additional detail: can only use the shadowed variable once inside the scope of the `if let`, not inside the expression
  * Downside of using if let: No exhaustive compiler checks
* `while let` conditional loops
  * Similar to `if let` but using a while, eg `while let Some(top) = stack.pop()`
* `for` loops
  * Deconstruction pattern can be used in for loops- `for (index, value) in v.iter().enumerate()`
* let
  * Technically let matches patterns- `let PATTERN = EXPRESSION`
  * eg destructuring is a pattern
  * Compiler enforces the numbers matching- eg `(x,y) = (1, 2, 3)` is compile error
* Function params
  * These are technically patterns, similar to let you can use destructuring on them

## Refutability

* An exhaustive pattern is irrefutable, ones that aren't exhaustive are not irrefutable
* `if let`, `while`, `for`, etc use refutable patterns while `match` uses irrefutable patterns
* Not a terribly important distinction, but worth knowing about so that you understand it when it appears in compiler messages
* Two messages they give as examples- error when using refutable pattern with `let`, warning when using irrefutable pattern with `if let`

## Pattern Syntax

* Need to be careful of shadowing variables in match statements- can get confusing
  * eg `let y = 10;... match x... Some(y) => ...` <- looks as if it's checking for `10` in `x`, but it isn't- it'll match any optional value and assign the result if it exists to a new variable `y`
* or
  * Can use pipe in match expressions, eg `match x { 1 | 2 => ... }` will match one or two
* Ranges
  * Uses `..=` eg `1..=5 => ...`
  * Also works with char- eg `'a'..='z'`
* Destructuring
  * Destructure structs using `let` by specifying the type to destructure
  * ```let p = Point { x: 0, y: 7 }; let Point { x: a, y: b } = p;```
  * Can also use the original fields and not specify names: ```let p = Point { x: 0, y: 7 }; let Point { x, y } = p;```
  * Can mix and match these to match cases where `x` is one variable or `y` is but keep the other
  * Can destructure enums in a similar way, eg `Message::Move { x, y }`
  * Can combine these to destructure nested objects- ```Message::ChangeColor(Color::Rgb(r, g, b)) => println!(
            "Change the color to red {}, green {}, and blue {}",
            r, g, b
        ),```
* Ignoring values in a pattern
  * If you want to ignore multiple parts of an object in a pattern (eg `{x,y,z}`) you can use `..` eg `({5, ..}`. This explains why the other syntax is `..=`.
  * Get first and last from iterator/list: `(first, .., last)`
* Match Guards
  * Additional `if` condition specified after pattern in a `match` arm
  * eg `Some(x) if x < 5 =>`
  * Match guards can compare values in pattern matching to variables declared outside match statement
  * `or` pipes don't apply to match guards; they are totally separate. ie `4 | 5 | 6 if y => ...` the `if y` applies to all patterns, not just `6`
* `@` bindings
  * Allows you to destructure variable and use them in a match arm, while also testing them
  * `Message:Hello { id: id_variable @ 3..=7, } => ...`
  * 