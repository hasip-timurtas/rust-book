# Writing Automated Tests

Rust's testing framework provides built-in unit and integration testing with zero external dependencies.

## Testing Philosophy

**Rust's type system** eliminates many bug categories (null pointer exceptions, type errors), but cannot verify business logic correctness.

**Testing fills the gap** between type safety and correctness verification.

## Built-in Testing Features

- **Unit tests**: `#[test]` annotation, same crate
- **Integration tests**: Separate `tests/` directory  
- **Documentation tests**: Code blocks in doc comments
- **Benchmarks**: Performance testing (nightly only)

## Compared to JavaScript Testing

**JavaScript**: External frameworks (Jest, Mocha, Vitest)
```javascript
// Requires dependencies
import { test, expect } from 'vitest';

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

**Rust**: Built-in testing
```rust
// No external dependencies
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

**Advantages**: 
- Zero setup required
- Integrated with `cargo test`
- Parallel execution by default
- Compile-time verification of test code
