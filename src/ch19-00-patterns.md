# Patterns and Matching

Patterns are syntactic constructs for matching against data structure shapes and extracting values. They work with `match` expressions, `if let`, `while let`, function parameters, and destructuring assignments.

Pattern components:
- **Literals**: `42`, `"hello"`, `true`
- **Destructuring**: `(a, b)`, `Some(x)`, `Point { x, y }`
- **Variables**: `x`, `mut counter`
- **Wildcards**: `_`, `..`
- **Guards**: `x if x > 0`

This chapter covers pattern usage contexts, refutability (whether patterns can fail to match), and comprehensive pattern syntax. Understanding patterns is essential for idiomatic Rust code involving data extraction and control flow.
