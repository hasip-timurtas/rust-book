# Functional Language Features: Iterators and Closures

Rust provides zero-cost functional programming abstractions that compile to efficient imperative code.

## Core Concepts

**Closures**: Anonymous functions with environment capture
- Similar to JavaScript arrow functions but with explicit capture semantics
- Compile-time optimization eliminates capture overhead

**Iterators**: Lazy evaluation chains with automatic optimization
- Functional composition compiles to imperative loops
- No performance penalty compared to manual iteration

## Performance Characteristics

**Zero-cost abstractions**: Functional style code performs identically to hand-optimized imperative code through compiler optimization.

**Compared to JavaScript**:

```javascript
// JavaScript - runtime overhead
const nums = [1, 2, 3, 4, 5];
const result = nums
  .filter(x => x > 2)
  .map(x => x * 2)
  .reduce((a, b) => a + b, 0);
```

```rust
// Rust - compiles to optimal loop
let nums = vec![1, 2, 3, 4, 5];
let result: i32 = nums
    .iter()
    .filter(|&x| x > 2)
    .map(|x| x * 2)
    .sum();
```

**Key advantage**: Expressive functional APIs with systems-level performance.

This chapter demonstrates building efficient, readable code through functional patterns while maintaining Rust's performance guarantees.
