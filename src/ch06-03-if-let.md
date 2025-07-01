## Concise Control Flow with `if let` and `let else`

`if let` provides sugar for `match` when you only care about one pattern, trading exhaustiveness checking for conciseness.

### Basic `if let` Syntax

Instead of verbose `match`:

```rust
let config_max = Some(3u8);
match config_max {
    Some(max) => println!("The maximum is configured to be {max}"),
    _ => (),
}
```

Use `if let`:

```rust
if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

### `if let` with `else`

```rust
let mut count = 0;
match coin {
    Coin::Quarter(state) => println!("State quarter from {state:?}!"),
    _ => count += 1,
}

// Equivalent with if let:
if let Coin::Quarter(state) = coin {
    println!("State quarter from {state:?}!");
} else {
    count += 1;
}
```

### `let...else` for Early Returns

Handle the "happy path" pattern cleanly:

```rust
// Before: nested conditions
fn describe_state_coin(coin: &Coin) -> String {
    if let Coin::Quarter(state) = coin {
        if state.year() < 1900 {
            format!("{state:?} is an old state!")
        } else {
            format!("{state:?} is a newer state.")
        }
    } else {
        "Not a state quarter.".to_string()
    }
}

// After: let...else for cleaner flow
fn describe_state_coin(coin: &Coin) -> String {
    let Coin::Quarter(state) = coin else {
        return "Not a state quarter.".to_string();
    };
    
    if state.year() < 1900 {
        format!("{state:?} is an old state!")
    } else {
        format!("{state:?} is a newer state.")
    }
}
```

### When to Use Each

**Use `match` when**:
- Need exhaustive handling
- Multiple patterns to handle
- Complex pattern matching

**Use `if let` when**:
- Only care about one pattern
- Optional else handling
- Want concise syntax

**Use `let...else` when**:
- Guard clause pattern (early return on mismatch)
- Want to continue with extracted value in main flow
- Avoiding nested conditions

### Performance

All three compile to identical code - choose based on readability and intent, not performance.

**Comparison to other languages**:

```javascript
// JavaScript - no compile-time safety
if (value !== null && value !== undefined) {
    console.log(value);
}
```

```rust
// Rust - compile-time safety with concise syntax
if let Some(value) = option_value {
    println!("{value}");
}
```

**Key insight**: These constructs provide syntactic flexibility while maintaining Rust's compile-time safety guarantees.

## Summary

We've now covered how to use enums to create custom types that can be one of a
set of enumerated values. We've shown how the standard library's `Option<T>`
type helps you use the type system to prevent errors. When enum values have
data inside them, you can use `match` or `if let` to extract and use those
values, depending on how many cases you need to handle.

Your Rust programs can now express concepts in your domain using structs and
enums. Creating custom types to use in your API ensures type safety: the
compiler will make certain your functions only get values of the type each
function expects.

In order to provide a well-organized API to your users that is straightforward
to use and only exposes exactly what your users will need, let's now turn to
Rust's modules.
