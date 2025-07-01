## Traits: Defining Shared Behavior

Traits define shared behavior contracts that types can implement. They're similar to interfaces in other languages but with some unique features.

### Defining a Trait

A trait groups method signatures that define required behavior:

<Listing number="10-12" file-name="src/lib.rs" caption="A `Summary` trait with a `summarize` method">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-12/src/lib.rs}}
```

</Listing>

The trait declaration uses the `trait` keyword followed by method signatures. Each implementing type must provide its own implementation.

### Implementing a Trait on a Type

<Listing number="10-13" file-name="src/lib.rs" caption="Implementing the `Summary` trait">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-13/src/lib.rs:here}}
```

</Listing>

Use the trait like any other method:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-01-calling-trait-method/src/main.rs}}
```

**Orphan rule**: You can only implement a trait on a type if either the trait or the type (or both) are local to your crate. This prevents conflicting implementations.

### Default Implementations

Traits can provide default method implementations:

<Listing number="10-14" file-name="src/lib.rs" caption="Default implementation">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-14/src/lib.rs:here}}
```

</Listing>

Use an empty `impl` block to accept defaults:

```rust,ignore
impl Summary for NewsArticle {}
```

Default implementations can call other trait methods:

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-03-default-impl-calls-other-methods/src/lib.rs:here}}
```

### Traits as Parameters

Use the `impl Trait` syntax for function parameters:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-04-traits-as-parameters/src/lib.rs:here}}
```

### Trait Bound Syntax

The full trait bound syntax is more verbose but more powerful:

```rust,ignore
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

Use trait bounds when you need multiple parameters of the same type:

```rust,ignore
pub fn notify<T: Summary>(item1: &T, item2: &T) {
```

### Multiple Trait Bounds

Specify multiple trait bounds with `+`:

```rust,ignore
pub fn notify(item: &(impl Summary + Display)) {
```

Or with generic syntax:

```rust,ignore
pub fn notify<T: Summary + Display>(item: &T) {
```

### Where Clauses

For complex trait bounds, use `where` clauses for clarity:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-07-where-clause/src/lib.rs:here}}
```

### Returning Types That Implement Traits

Return trait implementations without specifying concrete types:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-05-returning-impl-trait/src/lib.rs:here}}
```

**Limitation**: You can only return a single concrete type, not different types conditionally.

### Conditional Implementation

Implement methods conditionally based on trait bounds:

<Listing number="10-15" file-name="src/lib.rs" caption="Conditional method implementation">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-15/src/lib.rs}}
```

</Listing>

**Blanket implementations** implement a trait for any type that satisfies trait bounds:

```rust,ignore
impl<T: Display> ToString for T {
    // --snip--
}
```

This allows calling `to_string()` on any type implementing `Display`:

```rust
let s = 3.to_string();
```

Traits enable compile-time polymorphism while maintaining type safety and performance.
