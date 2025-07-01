## Characteristics of Object-Oriented Languages

Rust implements OOP concepts differently than traditional languages. Here's how Rust handles the core OOP characteristics:

### Objects: Data + Behavior

Rust structs and enums with `impl` blocks provide the same functionality as objects - they package data and methods together:

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-01/src/lib.rs}}
```

### Encapsulation

Rust uses `pub` keyword for visibility control. By default, everything is private:

<Listing number="18-1" file-name="src/lib.rs" caption="An `AveragedCollection` struct that maintains a list of integers and the average of the items in the collection">

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-01/src/lib.rs}}
```

</Listing>

Public methods control access to private data:

<Listing number="18-2" file-name="src/lib.rs" caption="Implementations of the public methods `add`, `remove`, and `average` on `AveragedCollection`">

```rust,noplayground
{{#rustdoc_include ../listings/ch18-oop/listing-18-02/src/lib.rs:here}}
```

</Listing>

Private fields prevent direct manipulation, ensuring data consistency. The internal implementation can change (e.g., from `Vec<i32>` to `HashSet<i32>`) without breaking external code.

### Inheritance: Not Supported

Rust has no traditional inheritance. Instead:

**For code reuse**: Use default trait implementations:
```rust
trait Summary {
    fn summarize(&self) -> String {
        "Default implementation".to_string()
    }
}
```

**For polymorphism**: Use trait objects or generics with trait bounds:
```rust
// Trait objects (dynamic dispatch)
fn process(item: &dyn Summary) { }

// Generics (static dispatch) 
fn process<T: Summary>(item: &T) { }
```

Rust's approach avoids inheritance issues like tight coupling and fragile base class problems. Trait objects enable polymorphism where different types can be treated uniformly if they implement the same trait.
