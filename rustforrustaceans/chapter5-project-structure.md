# Chapter 5, Project Structure

* Features
  * Enable or disable parts of your code or dependencies
  * Features must not be mutually exclusive
    * If `A` and `B` require `C` but require different features in `C` then Cargo will compile `C` once with the union of features required by `A` and `B`. If any combination of enabled features doesn't work, it may break someone's project
  * Recommends a tool called `cargo-hack` to be used in your CI pipeline to test that the project works with any combination of features enabled
* Using features cuts compile time significantly, by only compiling what is needed
* Use `cfg` to enable to disable features
* Comments that `tokio` uses a separate workspace for tests, `tokio-tests`, but I find that confusing looking at the actual source.
  * `tokio-tests` only has a small number of tests. Meanwhile, `tokio/src` stuff has tons of unit tests and `tokio/tests` has a great number of acceptance tests. So just what sort of tests are going into `tokio-tests` and why?
    * Probable answer: It seems the acceptance tests in `tokio/tests` are using `tokio-tests` dependencies. So it's a series of mocks and other test tools?
* Project configuration (things you can add to `cargo.toml`)
  * Project metadata
    * `description`, `homepage`, `readme` (path to the README file), default binary to run with `cargo run` (`default-run`), additional `keywords` and `categories` to help categorize your crate
    * `include` and `exclude` to define files to include/exclude from crate packing
    * "Manifest Format" page of Cargo reference to list all, new ones added frequently
  * `[profile]` performance options for compiler
    * `opt-level` - level of optimisation to go for, more optimised = slower build but maybe faster runtime. Use only in production mode
    * `lto`- another performance option, changes how the linkner works, increase build time but maybe faster runtime
* Minimal Dependency Versions
  * Recommends not using the latest of any dependency because it might cause problems with your consumer if other dependencies need an older version of your dependency
  * Recommends using the oldest version that has all the features you need
    * Seems mostly sensible but what about bug fixes and performance improvements?
  * Use `-Zminimal-versions` compile flag to make the compiler try to use the oldest fitting versions of each dependency. If it doesn't work increment them one at a time until you have the minimum viable dependency versions.
* Changelogs- https://keepachangelog.com
* 
