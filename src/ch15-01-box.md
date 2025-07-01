## Using `Box<T>` to Point to Data on the Heap

`Box<T>` allocates data on the heap with single ownership. Primary use cases:

- Types with unknown compile-time size (recursive types)
- Large data that should move without copying
- Trait objects (covered in Chapter 18)

### Basic Usage

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-01/src/main.rs}}
```

`Box<T>` provides heap allocation with automatic cleanup when it goes out of scope.

### Enabling Recursive Types

Recursive types require indirection since Rust needs to know type sizes at compile time.

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-02/src/main.rs:here}}
```

This fails because `List` has infinite size:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-03/output.txt}}
```

#### Size Calculation Issue

Non-recursive enums use the size of their largest variant. Recursive types create infinite size calculations since each `Cons` variant contains another `List`.

#### Solution with `Box<T>`

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-05/src/main.rs}}
```

`Box<T>` has a known size (pointer size), breaking the recursive chain. The heap-allocated data structure achieves the desired recursion without infinite compile-time size.

`Box<T>` implements `Deref` for reference-like behavior and `Drop` for automatic cleanup, making it a true smart pointer.
