# Chapter 14: More About Cargo and Crates

## Customising Builds with Release Profiles

* Cargo profiles are predefined but can be customised
* Main profiles are `dev` and `release`, which we've seen before
* Can customise them by adding `[profile.*]` settings to `cargo.toml`, eg:
  * ```[profile.dev]
opt-level = 0```

## Publishing a Crate to Crates.io

* Supports documentation
  * Like JDoc but without multiline comments
  * Just stick a comment above a function (or struct or mod?) to add a doc comment
  * Support Markdown
  * Generate docs with `cargo doc`, `cargo doc --open` to open them
* Common documentation headers- Examples, Panics, Errors, Safety
* `cargo test` will run the code samples in your documentation as tests
  * This is extremely cool
  * I can imagine it may result in the examples getting quite long and complicated though, having to supply the dependencies
* Other style of comment- `//!`
  * This is a comment about _the file it is in_ instead of the following item, like a regular doc comment.
  * This allows us to add doc comments to modules, whereas otherwise the comment would apply to the next function/struct
* Exporting a convenient public API- use re-exporting with `pub use` (as explained in an earlier chapter- 4?) to export the top-level items you want to
* Publishing on crates.io
  * Login w/ GitHub user
  * cargo publish
  * Publishing is _permanent_, version can never be removed
* `cargo yank` to mark a version of your library that should not be used- prevents `cargo add`ing it, but allows existing ones that already added it to continue using it.

## Cargo Workspaces

* Top-level `cargo.toml` defined with `[workspace]` that shares dependencies+config with subdirectory projects
* Can add one project as a dependency of another using relative paths in dependencies (ie `[dependencies]...localproj = { path = "../localproj"}`)
