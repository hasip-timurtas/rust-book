# Patterns and Matching

Patterns provide destructuring and matching capabilities for Rust's type system, enabling elegant data extraction and control flow.

## Pattern Components

- **Literals**: `42`, `"hello"`, `true`
- **Variables**: `x`, `name`, `_` (wildcard)
- **Destructuring**: `(a, b)`, `Point { x, y }`, `Some(value)`
- **Guards**: `x if x > 0`
- **Ranges**: `1..=5`, `'a'..='z'`

## Pattern Contexts

**`match` expressions**: Exhaustive pattern matching
**`if let`**: Conditional destructuring  
**Function parameters**: `fn process((x, y): (i32, i32))`
**`let` statements**: `let (first, rest) = tuple`

## Refutable vs Irrefutable

**Irrefutable**: Patterns that always match (`let x = 5`)
**Refutable**: Patterns that may fail (`if let Some(x) = option`)

## Compared to JavaScript Destructuring

**JavaScript**: Runtime destructuring
```javascript
const { name, age } = person;
const [first, ...rest] = array;
```

**Rust**: Compile-time pattern matching with exhaustiveness checking
```rust
let Person { name, age } = person;
let [first, rest @ ..] = array;
match result {
    Ok(value) => process(value),
    Err(e) => handle_error(e),
}
```

**Advantages**: 
- Compile-time exhaustiveness checking
- Type-safe destructuring
- Guard clauses for complex conditions

This chapter provides a comprehensive reference for Rust's pattern syntax and usage contexts.
