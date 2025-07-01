## Packages and Crates

### Crates

A _crate_ is Rust's compilation unit—the smallest code unit the compiler processes. Crates come in two forms:

- **Binary crates**: Executables with a `main` function (like Node.js applications)
- **Library crates**: Reusable code without `main` (like npm packages)

The _crate root_ is the source file where compilation begins, forming the root module of the crate's module tree.

### Packages

A _package_ contains one or more crates plus a `Cargo.toml` manifest. Think of it as similar to a Node.js project with `package.json`.

**Package rules:**
- Must contain at least one crate
- Can contain at most one library crate
- Can contain unlimited binary crates

### Cargo Conventions

```console
$ cargo new my-project
     Created binary (application) `my-project` package
```

Cargo follows these conventions:
- `src/main.rs` → binary crate root (package name)
- `src/lib.rs` → library crate root (package name)  
- `src/bin/` → additional binary crates (one per file)

A package with both `src/main.rs` and `src/lib.rs` contains two crates: a binary and library, both named after the package. This pattern is common for CLI tools that expose both an executable and library API.

[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
