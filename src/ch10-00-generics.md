# Generic Types, Traits, and Lifetimes

Rust's generics provide compile-time polymorphism through abstract type parameters. This chapter covers three key concepts: generic types for code reuse, traits for shared behavior contracts, and lifetimes for memory safety guarantees.

You've already used generics with `Option<T>`, `Vec<T>`, `HashMap<K, V>`, and `Result<T, E>`. Now you'll learn to define your own generic types, functions, and methods.

## Removing Duplication by Extracting a Function

Before exploring generics, let's examine the pattern of extracting common functionality. Consider finding the largest number in a list:

<Listing number="10-1" file-name="src/main.rs" caption="Finding the largest number in a list of numbers">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-01/src/main.rs:here}}
```

</Listing>

To handle multiple lists without code duplication, extract the logic into a function:

<Listing number="10-3" file-name="src/main.rs" caption="Abstracted code to find the largest number in two lists">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-03/src/main.rs:here}}
```

</Listing>

The refactoring process:
1. Identify duplicate code
2. Extract into function body with appropriate signature
3. Replace duplicated code with function calls

This same pattern applies to generics—replacing specific types with abstract placeholders that work across multiple types.
