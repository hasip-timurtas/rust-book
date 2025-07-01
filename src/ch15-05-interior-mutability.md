## `RefCell<T>` and the Interior Mutability Pattern

Interior mutability allows mutating data through immutable references by deferring borrow checking to runtime. `RefCell<T>` enforces borrowing rules at runtime instead of compile time.

### Runtime vs Compile-time Borrow Checking

| Type | Ownership | Borrow Check | Mutability |
|------|-----------|--------------|------------|
| `Box<T>` | Single | Compile-time | Immutable or mutable |
| `Rc<T>` | Multiple | Compile-time | Immutable only |
| `RefCell<T>` | Single | Runtime | Immutable or mutable |

`RefCell<T>` is single-threaded only. For multithreaded scenarios, use `Mutex<T>`.

### Interior Mutability Use Case

Testing scenario requiring mutation through immutable interface:

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-20/src/lib.rs}}
```

Attempting to implement a mock that tracks sent messages:

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-21/src/lib.rs:here}}
```

This fails because `send` takes `&self` but we need to mutate `sent_messages`:

```console
{{#include ../listings/ch15-smart-pointers/listing-15-21/output.txt}}
```

### Solution with `RefCell<T>`

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-22/src/lib.rs:here}}
```

`RefCell<T>` provides:
- `borrow()` returns `Ref<T>` (immutable smart pointer)
- `borrow_mut()` returns `RefMut<T>` (mutable smart pointer)

Both implement `Deref` for transparent access.

### Runtime Borrow Checking

`RefCell<T>` tracks active borrows at runtime. Violating borrowing rules causes panics:

```rust,editable,ignore,panics
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-23/src/lib.rs:here}}
```

```console
{{#include ../listings/ch15-smart-pointers/listing-15-23/output.txt}}
```

### Combining `Rc<T>` and `RefCell<T>`

Multiple ownership with mutability:

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-24/src/main.rs}}
```

Output:
```console
{{#include ../listings/ch15-smart-pointers/listing-15-24/output.txt}}
```

`Rc<RefCell<T>>` enables multiple owners who can all mutate the shared data, checked at runtime rather than compile time.
