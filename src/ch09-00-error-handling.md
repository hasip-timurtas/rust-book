# Error Handling

Rust categorizes errors into two types:

- **Recoverable errors** - Use `Result<T, E>` to handle expected failures (file not found, network timeout)
- **Unrecoverable errors** - Use `panic!` macro for bugs and invariant violations (array bounds, null pointer dereference)

Unlike languages with exceptions, Rust makes error handling explicit through the type system. This forces you to decide how to handle each potential failure point, improving reliability and preventing silent failures.

This chapter covers when to use `panic!` vs `Result<T, E>` and how to effectively propagate and handle errors.
