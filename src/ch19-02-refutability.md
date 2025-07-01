## Refutability: Whether a Pattern Might Fail to Match

Patterns are either **irrefutable** (always match) or **refutable** (can fail to match).

### Irrefutable Patterns
- `x` in `let x = 5`
- `(a, b)` in `let (a, b) = (1, 2)`
- Always succeed, can't fail

### Refutable Patterns  
- `Some(x)` in `if let Some(x) = value`
- Can fail if value doesn't match expected shape

### Context Requirements

**Irrefutable patterns only:**
- `let` statements
- Function parameters  
- `for` loops

**Refutable patterns accepted:**
- `if let` expressions
- `while let` expressions
- `match` arms (except final arm)

### Common Errors

**Using refutable pattern with `let`:**

<Listing number="19-8" caption="Attempting to use a refutable pattern with `let`">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-08/src/main.rs:here}}
```

</Listing>

Error:
```console
{{#include ../listings/ch19-patterns-and-matching/listing-19-08/output.txt}}
```

**Fix with `let...else`:**

<Listing number="19-9" caption="Using `let...else` and a block with refutable patterns instead of `let`">

```rust,editable
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-09/src/main.rs:here}}
```

</Listing>

**Using irrefutable pattern with `if let`:**

<Listing number="19-10" caption="Attempting to use an irrefutable pattern with `if let`">

```rust,editable
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-10/src/main.rs:here}}
```

</Listing>

Warning:
```console
{{#include ../listings/ch19-patterns-and-matching/listing-19-10/output.txt}}
```

**Summary:** Use the right pattern type for the context. The compiler enforces these rules to prevent logic errors.
