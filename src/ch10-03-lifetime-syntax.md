## Validating References with Lifetimes

Lifetimes ensure references remain valid for their required duration. Every reference has a lifetime—the scope where it's valid. Most lifetimes are inferred, but explicit annotation is required when relationships between reference lifetimes are ambiguous.

### Preventing Dangling References

Lifetimes prevent dangling references by ensuring referenced data outlives the reference:

<Listing number="10-16" caption="Attempted use of a reference whose value has gone out of scope">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-16/src/main.rs}}
```

</Listing>

This fails because `x` is destroyed before `r` is used:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-16/output.txt}}
```

### The Borrow Checker

The borrow checker compares scopes to validate borrows:

<Listing number="10-17" caption="Lifetime annotations showing the problem">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-17/src/main.rs}}
```

</Listing>

The reference `r` (lifetime `'a`) outlives the referenced data `x` (lifetime `'b`).

Fixed version:

<Listing number="10-18" caption="Valid reference with longer data lifetime">

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-18/src/main.rs}}
```

</Listing>

### Generic Lifetimes in Functions

Functions returning references need lifetime parameters when the compiler can't determine the relationship between input and output lifetimes:

<Listing number="10-20" file-name="src/main.rs" caption="Function returning a reference (doesn't compile)">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-20/src/main.rs:here}}
```

</Listing>

Error:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-20/output.txt}}
```

### Lifetime Annotation Syntax

Lifetime parameters start with an apostrophe and are typically lowercase:

```rust,ignore
&i32        // a reference
&'a i32     // a reference with explicit lifetime
&'a mut i32 // a mutable reference with explicit lifetime
```

### Lifetime Annotations in Function Signatures

Declare lifetime parameters in angle brackets:

<Listing number="10-21" file-name="src/main.rs" caption="Function with lifetime annotations">

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-21/src/main.rs:here}}
```

</Listing>

This signature means:
- Both parameters live at least as long as lifetime `'a`
- The returned reference will live at least as long as lifetime `'a`
- The actual lifetime is the smaller of the two input lifetimes

Example usage:

<Listing number="10-22" file-name="src/main.rs" caption="Valid usage with different lifetimes">

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-22/src/main.rs:here}}
```

</Listing>

Invalid usage:

<Listing number="10-23" file-name="src/main.rs" caption="Invalid usage—reference outlives data">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-23/src/main.rs:here}}
```

</Listing>

### Thinking in Terms of Lifetimes

Only specify lifetime parameters for references that affect the output. If a function always returns the first parameter, the second parameter doesn't need a lifetime annotation:

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-08-only-one-reference-with-lifetime/src/main.rs:here}}
```

Returning references to values created within the function creates dangling references:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-09-unrelated-lifetime/src/main.rs:here}}
```

### Lifetime Annotations in Struct Definitions

Structs holding references need lifetime annotations:

<Listing number="10-24" file-name="src/main.rs" caption="Struct with reference field">

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-24/src/main.rs}}
```

</Listing>

This ensures the struct instance can't outlive the reference it holds.

### Lifetime Elision

Three rules eliminate the need for explicit lifetime annotations in common cases:

1. **Input lifetimes**: Each reference parameter gets its own lifetime
2. **Single input**: If there's exactly one input lifetime, it's assigned to all outputs  
3. **Methods**: If one parameter is `&self` or `&mut self`, its lifetime is assigned to all outputs

Examples:

```rust,ignore
// Rule 1: fn foo<'a>(x: &'a i32)
fn first_word(s: &str) -> &str {

// Rule 2: fn foo<'a>(x: &'a i32) -> &'a i32  
fn first_word<'a>(s: &'a str) -> &'a str {
```

For multiple inputs without `self`, explicit annotations are required:

```rust,ignore
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &str {  // Error: can't determine output lifetime
```

### Lifetime Annotations in Method Definitions

Struct lifetime parameters must be declared after `impl`:

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-10-lifetimes-on-methods/src/main.rs:1st}}
```

Third elision rule example:

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-10-lifetimes-on-methods/src/main.rs:3rd}}
```

### The Static Lifetime

The `'static` lifetime indicates data living for the entire program duration:

```rust
let s: &'static str = "I have a static lifetime.";
```

String literals are stored in the binary and have `'static` lifetime. Use `'static` only when data truly lives for the program's entire duration.

### Generics, Trait Bounds, and Lifetimes Together

Example combining all three concepts:

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-11-generics-traits-and-lifetimes/src/main.rs:here}}
```

This function:
- Is generic over type `T` (with `Display` trait bound)
- Has lifetime parameter `'a` for references
- Ensures the returned reference lives as long as both inputs

Lifetimes provide compile-time memory safety guarantees without runtime overhead.
