## Method Syntax

Methods are functions defined within a struct's context using `impl` blocks. The first parameter is always `self`, representing the instance being called.

### Defining Methods

<Listing number="5-13" file-name="src/main.rs" caption="Defining an `area` method on the `Rectangle` struct">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-13/src/main.rs}}
```

</Listing>

**Key Points:**
- `impl Rectangle` creates an implementation block
- `&self` is shorthand for `self: &Self` where `Self` aliases the impl type
- Methods use dot notation: `rect1.area()`
- Borrowing rules apply: `&self` (immutable), `&mut self` (mutable), `self` (take ownership)

Using `&self` preserves ownership in the caller, similar to function parameters. Methods consuming `self` are rare and typically used for transformations.

### Methods vs Fields

Methods can have the same name as fields:

<Listing number="05-5" file-name="src/main.rs" caption="Example code in src/main.rs">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-06-method-field-interaction/src/main.rs:here}}
```

</Listing>

- `rect1.width()` calls the method
- `rect1.width` accesses the field

This pattern enables getter methods for controlled field access, supporting public methods with private fields.

### Automatic Referencing

Rust automatically handles referencing/dereferencing for method calls:

```rust
# #[derive(Debug,Copy,Clone)]
# struct Point {
#     x: f64,
#     y: f64,
# }
#
# impl Point {
#    fn distance(&self, other: &Point) -> f64 {
#        let x_squared = f64::powi(other.x - self.x, 2);
#        let y_squared = f64::powi(other.y - self.y, 2);
#
#        f64::sqrt(x_squared + y_squared)
#    }
# }
# let p1 = Point { x: 0.0, y: 0.0 };
# let p2 = Point { x: 5.0, y: 6.5 };
p1.distance(&p2);
(&p1).distance(&p2);  // Equivalent
```

This eliminates the need for manual `*` or `->` operators found in C/C++.

### Methods with Parameters

<Listing number="5-14" file-name="src/main.rs" caption="Using the as-yet-unwritten `can_hold` method">

```rust,ignore
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-14/src/main.rs}}
```

</Listing>

Expected output:
```text
Can rect1 hold rect2? true
Can rect1 hold rect3? false
```

Implementation:

<Listing number="5-15" file-name="src/main.rs" caption="Implementing the `can_hold` method on `Rectangle` that takes another `Rectangle` instance as a parameter">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-15/src/main.rs:here}}
```

</Listing>

Parameters after `self` work like regular function parameters. Use borrowing for parameters you don't need to own.

### Associated Functions

Functions in `impl` blocks without `self` are associated functions (similar to static methods):

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-03-associated-functions/src/main.rs:here}}
```

Called using `::` syntax: `Rectangle::square(3)`. Common for constructors like `String::from()`.

### Multiple `impl` Blocks

Structs can have multiple `impl` blocks:

<Listing number="5-16" caption="Rewriting Listing 5-15 using multiple `impl` blocks">

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-16/src/main.rs:here}}
```

</Listing>

Multiple blocks are useful with generics and traits (Chapter 10).

## Summary

Structs provide custom types with named fields and associated behavior through methods. Key concepts:

- **Field access**: Dot notation with borrowing considerations
- **Methods**: Functions with `self` parameter for instance behavior  
- **Associated functions**: Type-scoped functions for construction/utilities
- **`impl` blocks**: Organize type-related functionality
- **Automatic referencing**: Eliminates manual pointer dereferencing

Next: Enums provide another approach to custom types with variant-based data modeling.

[enums]: ch06-00-enums.html
[trait-objects]: ch18-02-trait-objects.md
[public]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
