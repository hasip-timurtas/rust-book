## Using Trait Objects for Runtime Polymorphism

Trait objects enable storing different types in the same collection when they implement a common trait. They provide dynamic dispatch at runtime cost.

### Basic Usage

Define a trait for common behavior:

<Listing number="18-3" file-name="src/lib.rs" caption="Definition of the `Draw` trait">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-03/src/lib.rs}}
```

</Listing>

Store trait objects using `Box<dyn Trait>`:

<Listing number="18-4" file-name="src/lib.rs" caption="Definition of the `Screen` struct with a `components` field holding a vector of trait objects that implement the `Draw` trait">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-04/src/lib.rs:here}}
```

</Listing>

Call methods on trait objects:

<Listing number="18-5" file-name="src/lib.rs" caption="A `run` method on `Screen` that calls the `draw` method on each component">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-05/src/lib.rs:here}}
```

</Listing>

### Trait Objects vs. Generics

**Generics** (monomorphization - static dispatch):
<Listing number="18-6" file-name="src/lib.rs" caption="An alternate implementation of the `Screen` struct and its `run` method using generics and trait bounds">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-06/src/lib.rs:here}}
```

</Listing>

- All components must be the same type
- Zero-cost abstraction (compile-time dispatch)
- Code duplication for each concrete type

**Trait Objects** (dynamic dispatch):
- Mixed types in same collection
- Runtime cost for method lookup
- Single implementation in binary

### Implementation Examples

<Listing number="18-7" file-name="src/lib.rs" caption="A `Button` struct that implements the `Draw` trait">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-07/src/lib.rs:here}}
```

</Listing>

<Listing number="18-8" file-name="src/main.rs" caption="Another crate using `gui` and implementing the `Draw` trait on a `SelectBox` struct">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch18-oop/listing-18-08/src/main.rs:here}}
```

</Listing>

<Listing number="18-9" file-name="src/main.rs" caption="Using trait objects to store values of different types that implement the same trait">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch18-oop/listing-18-09/src/main.rs:here}}
```

</Listing>

### Compile-time Safety

Rust enforces trait implementation at compile time:

<Listing number="18-10" file-name="src/main.rs" caption="Attempting to use a type that doesn't implement the trait object's trait">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch18-oop/listing-18-10/src/main.rs}}
```

</Listing>

```console
{{#include ../listings/ch18-oop/listing-18-10/output.txt}}
```

### Performance Trade-offs

**Static Dispatch** (generics):
- Faster execution (no runtime lookup)
- Larger binary size (code duplication)
- All types known at compile time

**Dynamic Dispatch** (trait objects):
- Runtime method lookup overhead
- Smaller binary size
- Enables heterogeneous collections
- Prevents some compiler optimizations

Choose based on your use case: use generics for performance-critical code with known types, trait objects for flexible APIs with mixed types.

[performance-of-code-using-generics]: ch10-01-syntax.html#performance-of-code-using-generics
[dynamically-sized]: ch20-03-advanced-types.html#dynamically-sized-types-and-the-sized-trait
[dyn-compatibility]: https://doc.rust-lang.org/reference/items/traits.html#dyn-compatibility
