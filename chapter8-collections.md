# Chapter 8 - Common Collections

* The most common collections: `Vector`, `String` and `HashMap`

## Storing Lists of Values with Vectors

* Vectors can only store values of the same type
* `let v: Vec<i32> = Vec::new();`
  * This is an uncommon way of declaring a vec
* Rust provides a macro for creating vectors that infers the type
  * `let v = vec![1, 2, 3];`
* Add to vector- `v.push(1);`
* Can also infer type by using `.push`- if you only push one type Rust will infer the type from that
* Two ways to access values from a vector- `&v[3]` and `v.get(3)`.
  * `get` returns an `Option<&T>`, but using `[3]` will panic if the value isn't found
* Can't modify a vector if you have a reference to an element in that vector
  * adding a new element onto the end of the vector might require allocating new memory and copying the old elements to the new space, if there isn’t enough room to put all the elements next to each other where the vector currently is. In that case, the reference to the first element would be pointing to deallocated memory.
* Can iterate through a vector with `for i in &v {...}`
* Can iterate through a vecotr mutably with `for i in &mut v { *i =... }`
  * Uses the _dereference operator_ `*` to mutate `i`
* Can contain different types in a vector by wrapping them with an enum
  * ```
      enum SpreadsheetCell {
        Int(i32),
        Float(f64),
        Text(String),
    }

    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];
    ```
* 

## Storing UTF8-Encoded Text with Strings

* `String` and `&Str`, as we've already seen
* Both always UTF-8 encoded
* Rust standard library also includes `OsString`, `OsStr`, `CString` and `CStr`. Seem to be used for interoperability with C and the OS.
* Can add strings together with `+` but the first must be modified and the rest must be references: `let s = s1 + "-" + &s2 + "-" + &s3;`
* Or better yet use the `format!` macro- `let s = format!("{}-{}-{}", s1, s2, s3);`
* Rust strings don't support indexing
* A string internally is a `Vec<u8>`.
  * Not all `u8` values refer to a single value, some will be part of multi-character unicode bytes
  * Indexing strings is disabled because this leads to confusion- a given index may be a value that is not a valid character
* **Do not use range index to split strings**- it can lead to a panic if you split halfw-way through a unicode character

## Storing Keys with Associated Values in Hash Maps

* Not in the prelude, needs to be imported
* Create a new hasmap- `HashMap::new();`
* Add values- `scores.insert(String::from("wat"), 10);`
* Infers types from `insert`
* Can initialise from a vector of tuples, where types are consistent
  * With `.zip` which creates a vector of tuples from two vectors, you can convert two vectors to a `HashMap`
* Get values out- `scores.get(&key)`
* Can iterate over using tuples- `for (key, value) in &scores {...}`
* Insert only if value doesn't already exist- `entry(&key).or_insert(50);`
  * `entry` returns enum `Entry`, similar to `Result`/`Option`
* `Entry` always contains a mutable reference so you can modify the element in the hashmap (leaving ownership to the hashmap) with eg `*value +=1 1;`
* 
