# Smart Pointers

Smart pointers are data structures that act like pointers but provide additional metadata and capabilities beyond standard references. While references (`&`) only borrow values with no overhead, smart pointers often own their data and implement specific memory management patterns.

Key smart pointers in Rust's standard library:

- `Box<T>` - heap allocation with single ownership
- `Rc<T>` - reference counting for multiple ownership (single-threaded)
- `Ref<T>` and `RefMut<T>` via `RefCell<T>` - runtime borrow checking

Smart pointers implement the `Deref` and `Drop` traits:
- `Deref` enables automatic dereferencing, allowing smart pointers to behave like references
- `Drop` customizes cleanup behavior when values go out of scope

This chapter covers interior mutability patterns and reference cycle prevention using `Weak<T>`.

## Core Concepts

**Interior Mutability**: Modifying data through immutable references using types like `RefCell<T>` that enforce borrowing rules at runtime rather than compile time.

**Reference Cycles**: Circular references that prevent memory cleanup, resolved using weak references (`Weak<T>`) that don't affect reference counts.
