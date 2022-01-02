# Chapter 6 - Enums and Pattern Matching

## Defining an Enum

```
enum IpAddrKind {
  V4,
  V6,
}
...
let four = IpAddrKind::V4;
let six - IpAddrKind::V6;
```

* Why `Kind`- is that a naming convention?
* Define methods that take an enum and pass either version in- similar to polymorphism/interfaces?
* Can associate variables with enums...
  ```
      enum IpAddr {
        V4(u8, u8, u8, u8),
        V6(String),
    }

    let home = IpAddr::V4(127, 0, 0, 1);

    let loopback = IpAddr::V6(String::from("::1"));
  ```
* Can also put struct instances inside an enum
* Can define methods on structs (specific structs, not KindOf struct types)

### Option

* nulls bad mkay
* Included in **prelude**, doesn't need to be imported
  * "Prelude" seems to refer to all standard library components that are imported by default


## The Match Control Flow Operator

* Can `match` on enums, eg `match coin { Coin::Penny => 1...}`
* Important terminology; each pattern-expression block within a `match` block is an _arm_
* Can get values out of enums to use in arm expressions: `Coin::Quarter(state) => println!("State quarter from {}!", state)`
  * Terminology: _Binding_, _bind_ the enum variable
* Ensures you check every possible case exhaustively

## Concise Control Flow with if let

* Easier syntax for one-arm match statements- `if let Some(3) = 3 {...}`
* `if let` takes a pattern and an expression separated by an equal sign. It's a match with one arm.
* However, you lose the exhaustive check the compiler enforces for `match`.
* Can have an `else` too
