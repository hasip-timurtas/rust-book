## Refactoring: Error Handling and Modularity

Current issues:
1. `main` function has multiple responsibilities
2. Configuration variables mixed with business logic
3. Generic error messages using `expect`
4. No input validation

### Binary Project Organization Pattern

Standard Rust pattern for CLI applications:
- **main.rs**: Command line parsing, configuration, calling `run()`, error handling
- **lib.rs**: Application logic, testable functions

### Extracting Configuration Parser

Extract argument parsing into a dedicated function:

<Listing number="12-5" file-name="src/main.rs" caption="Extracting `parse_config` function">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-05/src/main.rs:here}}
```

</Listing>

### Grouping Configuration Data

Use a struct instead of tuple for better semantics:

<Listing number="12-6" file-name="src/main.rs" caption="Refactoring to return `Config` struct">

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-06/src/main.rs:here}}
```

</Listing>

Note: Using `clone()` for simplicity. Chapter 13 covers more efficient approaches using iterators.

### Constructor Pattern

Convert to associated function following Rust conventions:

<Listing number="12-7" file-name="src/main.rs" caption="Converting to `Config::new`">

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-07/src/main.rs:here}}
```

</Listing>

### Error Handling

Replace `panic!` with proper error handling:

<Listing number="12-8" file-name="src/main.rs" caption="Adding argument validation">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-08/src/main.rs:here}}
```

</Listing>

### Returning Results

Better practice: return `Result` instead of panicking:

<Listing number="12-9" file-name="src/main.rs" caption="Returning `Result` from `Config::build`">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-09/src/main.rs:here}}
```

</Listing>

<Listing number="12-10" file-name="src/main.rs" caption="Handling errors in main">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-10/src/main.rs:here}}
```

</Listing>

Key pattern: `unwrap_or_else` with closure for custom error handling and controlled exit codes.

### Extracting Business Logic

Separate program logic from main:

<Listing number="12-11" file-name="src/main.rs" caption="Extracting `run` function">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-11/src/main.rs:here}}
```

</Listing>

### Propagating Errors

Make `run` return `Result` for proper error propagation:

<Listing number="12-12" file-name="src/main.rs" caption="Returning `Result` from `run`">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-12/src/main.rs:here}}
```

</Listing>

Using `Box<dyn Error>` trait object for flexible error types. The `?` operator propagates errors instead of panicking.

Handle the returned `Result` in main:

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/no-listing-01-handling-errors-in-main/src/main.rs:here}}
```

### Creating Library Crate

Move business logic to lib.rs for better testability:

<Listing number="12-13" file-name="src/lib.rs" caption="Defining search function signature">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-13/src/lib.rs}}
```

</Listing>

<Listing number="12-14" file-name="src/main.rs" caption="Using the library crate">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-14/src/main.rs:here}}
```

</Listing>

This structure enables:
- Unit testing of business logic
- Reusable library components  
- Clear separation of concerns
- Better error handling patterns

[ch13]: ch13-00-functional-features.html
[ch9-custom-types]: ch09-03-to-panic-or-not-to-panic.html#creating-custom-types-for-validation
[ch9-error-guidelines]: ch09-03-to-panic-or-not-to-panic.html#guidelines-for-error-handling
[ch9-result]: ch09-02-recoverable-errors-with-result.html
[ch18]: ch18-00-oop.html
[ch9-question-mark]: ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator
