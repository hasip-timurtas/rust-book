## Concise Control Flow with `if let` and `let else`

`if let` provides concise pattern matching for single patterns, trading exhaustiveness checking for brevity.

### Basic `if let` Syntax

Instead of verbose `match` for single-pattern cases:

<Listing number="6-6" caption="A `match` that only cares about executing code when the value is `Some`">

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-06/src/main.rs:here}}
```

</Listing>

Use `if let` for cleaner code:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-12-if-let/src/main.rs:here}}
```

**Syntax**: `if let pattern = expression { body }`

- Pattern matching works identically to `match`
- Only executes when pattern matches
- Loses exhaustiveness checking

### `if let` with `else`

Handle non-matching cases with `else`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-13-count-and-announce-match/src/main.rs:here}}
```

Equivalent using `if let`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-14-count-and-announce-if-let-else/src/main.rs:here}}
```

### `let...else` for Early Returns

`let...else` binds values or returns early, maintaining linear control flow:

<Listing number="6-7" caption="Checking whether a state existed in 1900 by using conditionals nested inside an `if let`.">

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-07/src/main.rs:describe}}
```

</Listing>

<Listing number="6-8" caption="Using `if let` to produce a value or return early.">

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-08/src/main.rs:describe}}
```

</Listing>

Cleaner with `let...else`:

<Listing number="6-9" caption="Using `let...else` to clarify the flow through the function.">

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-09/src/main.rs:describe}}
```

</Listing>

**`let...else` Properties:**
- Pattern matches: binds value in outer scope and continues
- Pattern fails: executes `else` block, which must diverge (return, panic, etc.)
- Maintains "happy path" flow without nested conditionals

### When to Use Each

| Pattern | Use Case | Trade-offs |
|---------|----------|------------|
| `match` | Multiple patterns, exhaustiveness required | Verbose for single patterns |
| `if let` | Single pattern, optional else case | No exhaustiveness checking |
| `let...else` | Extract value or early return | Must diverge on mismatch |

## Summary

Enums with pattern matching provide type-safe variant modeling:

- **Enums**: Model exclusive variants with associated data
- **`Option<T>`**: Eliminates null pointer errors through type safety
- **`match`**: Exhaustive pattern matching with data extraction
- **`if let`/`let...else`**: Concise syntax for common patterns

These constructs enable robust APIs where invalid states are unrepresentable, moving error handling from runtime to compile time.
