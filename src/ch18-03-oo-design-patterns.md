## Implementing Design Patterns in Rust

The state pattern encodes states as separate objects, with behavior changing based on internal state. This demonstrates two approaches: traditional OOP-style implementation and idiomatic Rust using the type system.

### Traditional State Pattern

The blog post workflow requires:
1. Draft → Review → Published transitions
2. Only published posts display content
3. Invalid transitions are ignored

<Listing number="18-11" file-name="src/main.rs" caption="Usage API for the blog post state pattern">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch18-oop/listing-18-11/src/main.rs:all}}
```

</Listing>

#### Implementation

<Listing number="18-12" file-name="src/lib.rs" caption="Definition of `Post` struct, `State` trait, and `Draft` struct">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-12/src/lib.rs}}
```

</Listing>

<Listing number="18-13" file-name="src/lib.rs" caption="Implementing the `add_text` method">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-13/src/lib.rs:here}}
```

</Listing>

<Listing number="18-14" file-name="src/lib.rs" caption="Placeholder `content` method returns empty string for drafts">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-14/src/lib.rs:here}}
```

</Listing>

#### State Transitions

<Listing number="18-15" file-name="src/lib.rs" caption="Implementing `request_review` methods">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-15/src/lib.rs:here}}
```

</Listing>

<Listing number="18-16" file-name="src/lib.rs" caption="Implementing the `approve` method">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-16/src/lib.rs:here}}
```

</Listing>

<Listing number="18-17" file-name="src/lib.rs" caption="Updating `content` method to delegate to state">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch18-oop/listing-18-17/src/lib.rs:here}}
```

</Listing>

<Listing number="18-18" file-name="src/lib.rs" caption="Adding the `content` method to the `State` trait">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-18/src/lib.rs:here}}
```

</Listing>

### Trade-offs of Traditional State Pattern

**Advantages:**
- State logic encapsulated in state objects
- Adding new states requires minimal changes
- Transitions managed internally

**Disadvantages:**
- States coupled to each other
- Logic duplication in transition methods
- Runtime overhead from trait objects
- Invalid states possible at runtime

### Type-Based State Pattern

Rust's type system can encode states as types, making invalid states impossible:

<Listing number="18-19" file-name="src/lib.rs" caption="Type-based approach with separate structs">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-19/src/lib.rs}}
```

</Listing>

<Listing number="18-20" file-name="src/lib.rs" caption="Implementing transitions as type transformations">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-20/src/lib.rs:here}}
```

</Listing>

<Listing number="18-21" file-name="src/main.rs" caption="Updated main function using type transformations">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch18-oop/listing-18-21/src/main.rs}}
```

</Listing>

### Type-Based Advantages

**Compile-time Safety:**
- Invalid states impossible
- Attempting to get content from draft fails to compile
- State transitions enforced by type system

**Performance:**
- No runtime state checks
- No trait object overhead
- Zero-cost abstractions

**API Design:**
- Clear ownership semantics
- Impossible to misuse API
- Self-documenting through types

### When to Use Each Pattern

**Traditional State Pattern:**
- Need runtime flexibility
- States determined by external data
- Working with existing OOP codebases

**Type-Based Pattern:**
- Compile-time state validation required
- Performance critical code
- Taking advantage of Rust's type system

The type-based approach demonstrates Rust's strength: encoding invariants in the type system prevents entire classes of bugs at compile time, with zero runtime cost.

[more-info-than-rustc]: ch09-03-to-panic-or-not-to-panic.html#cases-in-which-you-have-more-information-than-the-compiler
[macros]: ch20-05-macros.html#macros
