## Processing a Series of Items with Iterators

Iterators implement lazy evaluation—they do nothing until consumed. This enables efficient chaining of transformations without intermediate allocations.

<Listing number="13-10" file-name="src/main.rs" caption="Creating an iterator">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-10/src/main.rs:here}}
```

</Listing>

<Listing number="13-11" file-name="src/main.rs" caption="Using an iterator in a `for` loop">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-11/src/main.rs:here}}
```

</Listing>

### The `Iterator` Trait and `next` Method

All iterators implement the `Iterator` trait:

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // methods with default implementations elided
}
```

The associated type `Item` defines what the iterator yields. Only `next` requires implementation—all other methods have default implementations.

<Listing number="13-12" file-name="src/lib.rs" caption="Calling the `next` method on an iterator">

```rust,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-12/src/lib.rs:here}}
```

</Listing>

Note: `v1_iter` must be mutable because `next` consumes items. The `for` loop handles this automatically by taking ownership.

Iterator creation methods:
- `iter()`: Immutable references (`&T`)
- `into_iter()`: Owned values (`T`)  
- `iter_mut()`: Mutable references (`&mut T`)

### Consuming Adapters

Methods that call `next` and consume the iterator:

<Listing number="13-13" file-name="src/lib.rs" caption="Using `sum` to consume an iterator">

```rust,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-13/src/lib.rs:here}}
```

</Listing>

### Iterator Adapters

Methods that transform iterators into other iterators. Must be consumed to execute:

<Listing number="13-14" file-name="src/main.rs" caption="Iterator adapter `map` (produces warning)">

```rust,not_desired_behavior
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-14/src/main.rs:here}}
```

</Listing>

```console
{{#include ../listings/ch13-functional-features/listing-13-14/output.txt}}
```

<Listing number="13-15" file-name="src/main.rs" caption="Using `map` and `collect` together">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-15/src/main.rs:here}}
```

</Listing>

### Closures with Environment Capture

Iterator adapters commonly use closures that capture environment variables:

<Listing number="13-16" file-name="src/lib.rs" caption="Using `filter` with environment capture">

```rust,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-16/src/lib.rs}}
```

</Listing>

The `filter` closure captures `shoe_size` from the environment, demonstrating how iterators can access external state while maintaining functional programming patterns.
