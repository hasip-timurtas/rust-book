## References and Borrowing

References allow using values without taking ownership. A reference is guaranteed to point to a valid value for its lifetime.

<Listing number="4-5" file-name="src/main.rs" caption="A function that takes a reference as a parameter">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-07-reference/src/main.rs:all}}
```

</Listing>

The `&` syntax creates a reference. `&s1` refers to `s1` without owning it. When the reference goes out of scope, the value isn't dropped because the reference doesn't own it.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-08-reference-with-annotations/src/main.rs:here}}
```

Creating a reference is called _borrowing_. References are immutable by default:

<Listing number="4-6" file-name="src/main.rs" caption="Attempting to modify a borrowed value">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-06/src/main.rs}}
```

</Listing>

### Mutable References

Use `&mut` for mutable references:

<Listing number="4-7" file-name="src/main.rs" caption="Using a mutable reference to change a value">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-09-fixes-listing-04-06/src/main.rs}}
```

</Listing>

**Borrowing rules:**
- Only one mutable reference per value at a time
- Cannot mix mutable and immutable references in overlapping scopes

This prevents data races at compile time:

<Listing number="4-8" file-name="src/main.rs" caption="Attempting to create two mutable references">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-10-multiple-mut-not-allowed/src/main.rs:here}}
```

</Listing>

Multiple mutable references are allowed in separate scopes:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-11-muts-in-separate-scopes/src/main.rs:here}}
```

Cannot combine mutable and immutable references:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-12-immutable-and-mutable-not-allowed/src/main.rs:here}}
```

Reference scopes end at their last usage, not at the end of the block:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-13-reference-scope-ends/src/main.rs:here}}
```

### Dangling References

Rust prevents dangling references through compile-time checks:

<Listing number="4-9" file-name="src/main.rs" caption="Attempting to create a dangling reference">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-14-dangling-reference/src/main.rs}}
```

</Listing>

Error: function returns a borrowed value with no value to borrow from.

Solution—return the value directly:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-16-no-dangle/src/main.rs:here}}
```

### The Rules of References

- At any time: either one mutable reference OR any number of immutable references
- References must always be valid
