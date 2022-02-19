# Chapter 7- macros

* Macros are code that writes code- code generation / generative code.
* Two types- declarative macros and procedural macros
* What are declarative macros
  * Work like a function, called by `!` syntax, get replaced by result of macro
  * Good for when you're writing the same code repeatedly and want to reduce duplication
  * Eg implementing a trait on lots of structs
* How declarative macros work
  * Tokens and grammar- when parsed a source file is broken up into _tokens_ following the rules of a _grammar_
  * Tokens get arranged into an abstract syntax tree
  * Declarative macros work at that level- they are code that declare the syntax tree to be used in place of their callsite
  * Can use syntax that is not valid Rust syntax in a macro call, but have to follow the low-level rules of how the compiler deliminates tokens- eg can't leave a brace open without a closing brace
  * Declarative macro must produce valid Rust code
    * Otherwise, the compiler will not compile the macro definition itself
* How to write declarative macros
  * Consist of two parts...
  * _matchers_ - 
    * one to many, each associated with a transcriber- `() => {}, () => {}`
    * Compiler picks the first one that matches the invocation
    * Very expressive token language not unlike regular expressions
    * Define _fragment types_ with colon- eg `$a:ident + $b:expr` matches any identifier followed by `+` followed by an expression, eg `x + 3 * 5`
      * Other fragment types: `:ty` for types, `:tt` for any single token tree.
    * `+` denotes "one or more" (and may need to be used with `$()` groups?), eg
      * Match one or more comma-separated key/value pairs in `key => value` format: `$($key:expr => $value:expr),+`
  * _transcribers_
    * Transcriber creates the syntax tree for the matching matcher
    * Variables captured in matcher are called _metavariables_
    * Syntax is mostly regular Rust with `$metavariable` substitutions
    * However, to handle "one or more" matchers can use similar syntax to the matcher syntax:
      * `$(map.insert($key, $value);)+`
      * Cannot do one of these loops without using a metavariable. Otherwise, if you have more than one loop in the matcher the compiler won't know which one to use.
  * Hygenic
    * The concept of hygiene in macros means that there can be no conflicts between variable names in the macro output or outside its scope
    * eg if you have a variable `x` and your macro also outputs some code declaring a variable `x`, the new variable won't shadow the old one
    * Can think of them as a totally separate scope
    * However, hygiene does not apply to other concepts- structs, modules, functions, et al are not hygenic in macros because it is convenient to be able to declare new functions etc from a macro
    * When _exporting_ modules you have to be wary of namespaces- you can't be sure that the consumer has imported the namespaces so you need to fully specify every namespace!
  * They are not hoisted, i.e. only call them after they've been evaluated and not before
* What are procedural macros
  * Rather than writing all new tokens, modifies existing tokens. The compilers gathers the sequence of input tokens to the macro and runs your program to figure out what tokens to replace them with.
  * Basically a source code preprocessor that can arbitrarily modify tokens
* Types of procedural macros
  * Function-like macros, called with `!` similar to declarative macros
    * Very similar to declarative macros but less constraints / more powerful
    * No hygiene guarantees, instead you need to explicitly specify which identifiers should overlap the surrounding code and which ones should be treated as having a private scope
  * Attribute macros, like `#[test]`
    * Replaces the thing the attribute is attached to wholesale, but takes two arguments: the arguments to the attribute, and the thing it is attached to.
  * Derive macros, like `#[derive(Serialize)]`
    * Adds to, rather than replaces, the target
    * Have more guard rails than other procedural macros
* Procedural macros can significantly increase compile time, so use with caution
* When to use derive macros
  * When you want to automate deriving a trait, and it is obvious how that trait will behave when implemented
* When When to use function-like macros
  * When a declarative macro has gotten too out of hand, too hairy/complex
  * When you have a pure function need to execute at compile-time but cannot express it with `const fn`
    * Something that transforms data at compile-time (may depend on configuration?)
* When to use attribute macros
  * When generating a series of similar tests (parameterised tests?)
  * To reduce consumer boilerplate
    * Eg in webserver using an annotation to define a path and auto-injecting the path variables (akin to ASP.NET) rather than requiring the user to configure it elsewhere
  * To augment functions in unobtrusive ways that do not change the function's functionality
    * Eg adding attributes to log or collect metrics on a given function
  * To transform a type
    * I'm a bit unclear on what they mean by this one- but generally seems to refer to adding new functionality to a type, a bit more complex than the other examples
* How do procedural macros work?
  * `TokenStream`, which is a collection of `TokenTree` (recursive token node)
  * `syn` crate very useful for parsing the incoming `TokenStream`
  * 
