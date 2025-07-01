## Running Code on Cleanup with the `Drop` Trait

The `Drop` trait customizes cleanup behavior when a value goes out of scope. It's essential for smart pointers that manage resources like memory, files, or network connections.

### Implementing `Drop`

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-14/src/main.rs}}
```

The `Drop` trait requires implementing a `drop` method that takes `&mut self`. Rust automatically calls `drop` when values go out of scope.

Output:
```console
{{#include ../listings/ch15-smart-pointers/listing-15-14/output.txt}}
```

Variables are dropped in reverse order of creation (`d` before `c`).

### Manual Cleanup with `std::mem::drop`

You cannot call the `Drop` trait's `drop` method directly:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-15/src/main.rs:here}}
```

```console
{{#include ../listings/ch15-smart-pointers/listing-15-15/output.txt}}
```

Use `std::mem::drop` for early cleanup:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-16/src/main.rs:here}}
```

Output:
```console
{{#include ../listings/ch15-smart-pointers/listing-15-16/output.txt}}
```

`std::mem::drop` takes ownership of the value, triggering the `Drop` implementation immediately. This prevents double-free errors since Rust can't call `drop` again on a moved value.
