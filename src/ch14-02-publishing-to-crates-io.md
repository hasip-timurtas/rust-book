## Publishing a Crate to Crates.io

We've used packages from [crates.io](https://crates.io/)<!-- ignore --> as
dependencies of our project, but you can also share your code with other people
by publishing your own packages. The crate registry at
[crates.io](https://crates.io/)<!-- ignore --> distributes the source code of
your packages, so it primarily hosts code that is open source.

Rust and Cargo have features that make your published package easier for people
to find and use. We'll talk about some of these features next and then explain
how to publish a package.

### Documentation Comments

Use `///` for API documentation that generates HTML:

<Listing number="14-1" file-name="src/lib.rs" caption="Documentation comment example">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-01/src/lib.rs}}
```

</Listing>

Generate docs: `cargo doc --open`

#### Standard Sections

- **Panics**: Conditions that cause panics
- **Errors**: Error types and causes for `Result` returns  
- **Safety**: Safety requirements for `unsafe` functions

#### Documentation Tests

Code in documentation comments runs as tests with `cargo test`:

```text
   Doc-tests my_crate

running 1 test
test src/lib.rs - add_one (line 5) ... ok
```

#### Module Documentation

Use `//!` for crate/module-level documentation:

<Listing number="14-2" file-name="src/lib.rs" caption="Crate-level documentation">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-02/src/lib.rs:here}}
```

</Listing>

### Public API Design with `pub use`

Re-export items to create a clean public API regardless of internal structure:

<Listing number="14-3" file-name="src/lib.rs" caption="Internal module organization">

```rust,editable,noplayground,test_harness
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-03/src/lib.rs:here}}
```

</Listing>

Without re-exports, users must write:
```rust,editable,ignore
use art::kinds::PrimaryColor;
use art::utils::mix;
```

<Listing number="14-5" file-name="src/lib.rs" caption="Re-exporting for clean API">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-05/src/lib.rs:here}}
```

</Listing>

Now users can write:
```rust,editable,ignore
use art::{PrimaryColor, mix};
```

### Publishing Workflow

1. **Create account**: Log in at [crates.io](https://crates.io/) with GitHub
2. **Get API token**: From account settings, store with `cargo login`
3. **Add metadata** to `Cargo.toml`:

```toml
[package]
name = "your_crate_name"
version = "0.1.0"
edition = "2024"
description = "A brief description of your crate's functionality."
license = "MIT OR Apache-2.0"
```

4. **Publish**: `cargo publish`

#### Version Management

- **New versions**: Update `version` in `Cargo.toml`, run `cargo publish`
- **Yanking**: `cargo yank --vers 1.0.1` (prevents new usage, doesn't break existing)
- **Un-yanking**: `cargo yank --vers 1.0.1 --undo`

**Note**: Published versions are permanent and cannot be deleted.

### License Options

Use [SPDX identifiers](https://spdx.org/licenses/). Common choices:
- `MIT`: Permissive
- `Apache-2.0`: Permissive with patent grant
- `MIT OR Apache-2.0`: Rust ecosystem standard

For custom licenses, use `license-file` instead of `license`.

[spdx]: https://spdx.org/licenses/
[semver]: https://semver.org/
