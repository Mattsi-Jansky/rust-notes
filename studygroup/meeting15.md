# Meeting 15- Advanced Rust

## Jocelyn presenting first 2 sections

* Very cool animation on the presentation :D 
* Any external language interop requires `unsafe`. Makes sense because no other languages have the borrow rules
  * `#[no_mangle]` - attribute tells compiler not to mangle name of extern functions
* Concurrency: Implementing `Send` or `Sync` requires `unsafe`
* 
