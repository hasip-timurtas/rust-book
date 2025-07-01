## Reference Cycles Can Leak Memory

`Rc<T>` and `RefCell<T>` can create reference cycles where items refer to each other, preventing cleanup since reference counts never reach zero.

### Creating a Reference Cycle

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-25/src/main.rs}}
```

Creating circular references:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-26/src/main.rs:here}}
```

Output:
```console
{{#include ../listings/ch15-smart-pointers/listing-15-26/output.txt}}
```

Both `a` and `b` have reference count 2. When they go out of scope, counts drop to 1 but never reach 0, causing a memory leak.

### Preventing Reference Cycles Using `Weak<T>`

`Weak<T>` provides non-owning references that don't affect reference counts.

- `Rc::downgrade` creates `Weak<T>` from `Rc<T>`
- `Weak::upgrade` returns `Option<Rc<T>>`
- Strong references control cleanup; weak references don't

### Tree Data Structure Example

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-27/src/main.rs:here}}
```

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-27/src/main.rs:there}}
```

Adding parent references with `Weak<T>`:

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-28/src/main.rs:here}}
```

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-28/src/main.rs:there}}
```

### Reference Count Monitoring

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-29/src/main.rs:here}}
```

- `Rc::strong_count` tracks owning references
- `Rc::weak_count` tracks non-owning references
- Only strong count reaching zero triggers cleanup

When `branch` goes out of scope, its strong count drops to 0 and gets cleaned up despite having weak references pointing to it.

## Summary

Smart pointers provide different memory management patterns:

- `Box<T>`: Single ownership, heap allocation
- `Rc<T>`: Multiple ownership via reference counting
- `RefCell<T>`: Interior mutability with runtime borrow checking
- `Weak<T>`: Non-owning references to prevent cycles

Use `Weak<T>` for parent-child relationships where children should not own their parents, preventing reference cycles and memory leaks.
