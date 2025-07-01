## Treating Smart Pointers Like Regular References with `Deref`

The `Deref` trait customizes the dereference operator (`*`) behavior, allowing smart pointers to behave like regular references.

### Basic Dereferencing

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-06/src/main.rs}}
```

### Using `Box<T>` Like a Reference

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-07/src/main.rs}}
```

`Box<T>` can be dereferenced like a regular reference due to its `Deref` implementation.

### Implementing Custom Smart Pointers

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-08/src/main.rs:here}}
```

This `MyBox<T>` type won't work with the dereference operator without implementing `Deref`:

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-09/src/main.rs:here}}
```

### Implementing the `Deref` Trait

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-10/src/main.rs:here}}
```

The `deref` method returns a reference to the inner data. When you write `*y`, Rust actually executes `*(y.deref())`.

### Deref Coercion

Deref coercion automatically converts references to types implementing `Deref` into references to other types. This enables seamless interaction between different smart pointer types.

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-11/src/main.rs:here}}
```

Calling a function expecting `&str` with `&MyBox<String>`:

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-12/src/main.rs:here}}
```

Rust automatically applies deref coercion: `&MyBox<String>` → `&String` → `&str`.

Without deref coercion, you'd need explicit conversions:

```rust,editable
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-13/src/main.rs:here}}
```

### Deref Coercion Rules

Rust performs deref coercion in three cases:

1. `&T` to `&U` when `T: Deref<Target=U>`
2. `&mut T` to `&mut U` when `T: DerefMut<Target=U>`
3. `&mut T` to `&U` when `T: Deref<Target=U>`

Note: Immutable references cannot coerce to mutable references due to borrowing rules.
