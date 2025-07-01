## Test Organization

Rust categorizes tests as **unit tests** (small, focused, test one module in isolation, can test private interfaces) and **integration tests** (external to your library, use public API only, test multiple modules together).

### Unit Tests

Place unit tests in each file with the code they test, in a `tests` module annotated with `#[cfg(test)]`.

#### The Tests Module and `#[cfg(test)]`

The `#[cfg(test)]` annotation compiles and runs test code only with `cargo test`, not `cargo build`:

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-01/src/lib.rs}}
```

#### Testing Private Functions

Rust allows testing private functions since tests are just Rust code in the same crate:

<Listing number="11-12" file-name="src/lib.rs" caption="Testing a private function">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-12/src/lib.rs}}
```

</Listing>

The `use super::*;` brings parent module items into scope.

### Integration Tests

Create integration tests in a `tests` directory at the project root (alongside `src`). Each file in `tests` is compiled as a separate crate.

#### The _tests_ Directory

Project structure:
```text
adder
├── Cargo.lock
├── Cargo.toml
├── src
│   └── lib.rs
└── tests
    └── integration_test.rs
```

<Listing number="11-13" file-name="tests/integration_test.rs" caption="Integration test">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-13/tests/integration_test.rs}}
```

</Listing>

Each test file gets its own section in test output. Run specific integration test file:

```console
$ cargo test --test integration_test
```

#### Submodules in Integration Tests

Files in `tests` directory are separate crates and don't share the same behavior as `src` files. 

For shared helper functions, use subdirectories instead of top-level files:

```text
├── tests
    ├── common
    │   └── mod.rs      // Shared code
    └── integration_test.rs
```

Use from integration tests:

```rust,editable,ignore
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-13-fix-shared-test-code-problem/tests/integration_test.rs}}
```

#### Integration Tests for Binary Crates

Binary crates with only `src/main.rs` cannot have integration tests that import functions with `use` statements. Structure binary projects with logic in `src/lib.rs` and a thin `src/main.rs` that calls the library functions.

## Summary

Rust's testing features support both detailed unit testing and broader integration testing. Unit tests verify individual modules (including private functions), while integration tests validate public API behavior. This testing strategy helps ensure code correctness as you refactor and extend functionality.
