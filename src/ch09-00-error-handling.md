# Error Handling

Rust enforces explicit error handling through the type system, eliminating runtime exceptions.

## Error Categories

**Recoverable Errors**: `Result<T, E>` for operations that may fail
- File I/O, network requests, parsing
- Compositional error handling with `?` operator
- Similar to Promise chains in TypeScript

**Unrecoverable Errors**: `panic!` for programming bugs
- Array out-of-bounds, assertion failures
- Terminates program immediately
- Similar to throwing in critical paths

## Compared to Exception-Based Languages

**JavaScript/TypeScript**: Runtime exceptions, try-catch blocks
```javascript
try {
    const data = JSON.parse(input);
    return processData(data);
} catch (error) {
    console.error(error);
    return null;
}
```

**Rust**: Compile-time error handling
```rust
match serde_json::from_str(input) {
    Ok(data) => process_data(data),
    Err(e) => {
        eprintln!("Error: {}", e);
        return Err(e);
    }
}
```

**Benefits**: 
- All error paths explicitly handled at compile time
- No hidden exceptions
- Zero-cost abstractions for error propagation
