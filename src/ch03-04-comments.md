## Comments

Rust uses `//` for line comments and `/* */` for block comments:

```rust,editable
// Line comment
/* Block comment */
```

Multi-line comments require `//` on each line:

```rust,editable
// Multi-line comment
// continues here
```

Comments can be placed at line end or above code:

```rust,editable
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

```rust,editable
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

Documentation comments (`///` and `//!`) are covered in [Chapter 14][publishing].

[publishing]: ch14-02-publishing-to-crates-io.html
