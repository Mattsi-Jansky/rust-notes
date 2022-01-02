# Chapter 5 - Using Structs to Structure Related Data

## Defining and Instantiating Structs

* Structs are another data structure, similar to tuples
  * However, the elements are named
  * Elements in a struct are called _fields_
* Declaring a struct syntax:
  ```
  struct User {
      username: String,
      email: String,
      sign_in_count: u64,
      active: bool
  }
  ```
* To use a struct create an _instance_ of it it:
```
let user1 = User { 
  email: String::from("someone@example.com"),
  username: String::from("someusername123"),
  active: true,
  sign_in_count: 1
};
```
  * Fields don't have to be in their original order
* Can instantiate mutable structs: `let mut user1 = User {...}`
  * Change with `user1.email = ...`
* If your variable is the same name as the struct field name, you don't have to specify the field name to instantiate the struct
```
  User {
      email,
      username,
      active: true,
      sign_in_count: 1,
  }
```
* Struct Update Syntax
  * Instantiate a new struct that uses most of an old instance's values
  ```
    let user2 = User{
      email: String::from("another@example.com"),
      username: String::from("anotherusername567"),
      ..user1
    }
  ```

### Using Tuple Structs without Named Fields to Create Different Types

* _Tuple structs_- structs, but with syntax similar to a tuple. 
```
struct Color(i32, i32, i32)
struct Point(i32, i32, i32)


let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```
  * Useful when creating a whole new struct would be overly verbose/redundant
  * Gives type safety- method expecting a `Color` won't accept a `Point`
* 

### Unit-like Structs Without Any Fields

* Can also define structs that don't have any fields, _unit-like structs_ because they behave similarly to the unit (null) type, `()`.
  * Apparently this is useful for implementing traits on a struct that doesn't contain any actual fields within itself
  * More on this in chapter 10

## An Example Program Using Structs

* Started with passing variables into a method, then moved to using tuples, then to using structs

### Method Syntax

```
impl Rectangle {
  fn area(&self) -> u32 {
    self.width * self.height
  }
}
...
rect.area()
```

* `impl` is an implementation block
* Includes `&self` parameter
  * Why? It'd be nicer to exclude / add it in the background automatically. I suppose it's because you need to specify the type of variable- reference, mutable referenced or take ownership
* 

### Associated Functions

* Static methods, basically. Functions within `impl` blocks that don't take `self` as a param.
* Can have multiple `impl` blocks for one `struct`
* Yet another reference to chapter 10- sounds like a big, scary chapter with advanced concepts and a lot of stuff at this level is getting put off to then!