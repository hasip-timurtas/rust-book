## Improving Our I/O Project

This section demonstrates refactoring imperative code to use iterators, eliminating `clone` calls and improving readability through functional patterns.

### Removing `clone` with Iterator Ownership

Original implementation with inefficient cloning:

<Listing number="13-17" file-name="src/main.rs" caption="Original `Config::build` with cloning">

```rust,ignore
{{#rustdoc_include ../listings/ch13-functional-features/listing-12-23-reproduced/src/main.rs:ch13}}
```

</Listing>

#### Refactoring to Accept an Iterator

Change `main.rs` to pass the iterator directly:

<Listing number="13-18" file-name="src/main.rs" caption="Passing iterator to `Config::build`">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-18/src/main.rs:here}}
```

</Listing>

Update the function signature to accept any iterator returning `String`:

<Listing number="13-19" file-name="src/main.rs" caption="Updated signature accepting iterator">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-19/src/main.rs:here}}
```

</Listing>

#### Using Iterator Methods Instead of Indexing

Replace indexing with `next()` calls:

<Listing number="13-20" file-name="src/main.rs" caption="Using iterator methods">

```rust,ignore,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-20/src/main.rs:here}}
```

</Listing>

This eliminates cloning by taking ownership of iterator values, while improving error handling with explicit `None` checks.

### Functional Style with Iterator Adapters

Original imperative search function:

<Listing number="13-21" file-name="src/lib.rs" caption="Imperative search implementation">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-19/src/lib.rs:ch13}}
```

</Listing>

Functional refactor using iterator adapters:

<Listing number="13-22" file-name="src/lib.rs" caption="Functional search using iterators">

```rust,ignore
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-22/src/lib.rs:here}}
```

</Listing>

Benefits:
- Eliminates mutable state (`results` vector)
- Enables potential parallelization 
- More concise and declarative
- Better composability

**Performance note**: Both implementations compile to equivalent machine code due to Rust's zero-cost abstractions.

<!-- Old heading. Do not remove or links may break. -->

<a id="choosing-between-loops-or-iterators"></a>

### Iterator vs. Loop Performance

Iterator adapters are zero-cost abstractions—they compile to the same assembly as hand-written loops. The functional style often provides better readability and composability without performance penalties.

For high-performance applications, prefer iterators for their:
- Compile-time optimizations (loop unrolling, bounds check elimination)
- Reduced cognitive load through declarative style
- Built-in parallelization support with external crates

[impl-trait]: ch10-02-traits.html#traits-as-parameters
