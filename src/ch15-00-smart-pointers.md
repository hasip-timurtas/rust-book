# Smart Pointers

Smart pointers provide automatic memory management with explicit ownership semantics, enabling safe shared ownership and interior mutability patterns.

## Concept Overview

**Regular References**: Borrow data, no ownership
**Smart Pointers**: Own data, implement `Deref` and `Drop` traits

Examples you've used: `String`, `Vec<T>` are smart pointers with heap allocation and metadata.

## Key Smart Pointers

**`Box<T>`**: Heap allocation for owned data
- Single ownership, no runtime overhead
- Enables recursive types and heap allocation

**`Rc<T>`**: Reference counting for shared ownership  
- Multiple immutable owners
- Runtime overhead for reference counting

**`RefCell<T>`**: Interior mutability with runtime borrow checking
- Mutable access through immutable references
- Panics on borrow rule violations

## Compared to JavaScript References

**JavaScript**: Garbage-collected references
```javascript
let obj = { data: 42 };
let ref1 = obj;  // Both point to same object
let ref2 = obj;  // Garbage collected when no references remain
```

**Rust**: Explicit ownership with compile-time safety
```rust
let data = Rc::new(42);
let ref1 = Rc::clone(&data);  // Explicit shared ownership
let ref2 = Rc::clone(&data);  // Reference counted, no GC needed
```

## Pattern Applications

**Interior Mutability**: Mutating through immutable references when safe
**Reference Cycles**: Potential memory leaks and prevention strategies
**Custom Smart Pointers**: Building domain-specific ownership patterns

This chapter covers advanced memory management patterns while maintaining Rust's safety guarantees.
