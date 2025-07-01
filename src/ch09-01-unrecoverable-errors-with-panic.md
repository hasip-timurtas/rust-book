## Unrecoverable Errors with `panic!`

The `panic!` macro immediately terminates the program when encountering unrecoverable errors. By default, it unwinds the stack, cleaning up data from each function before exiting.

### Panic Behavior Configuration

```toml
[profile.release]
panic = 'abort'  # Skip unwinding, let OS clean up (smaller binary)
```

### Explicit Panic

```rust,should_panic,panics
fn main() {
    panic!("crash and burn");
}
```

Output:
```
thread 'main' panicked at 'crash and burn', src/main.rs:2:5
```

### Panic from Invalid Operations

```rust,should_panic,panics
fn main() {
    let v = vec![1, 2, 3];
    v[99]; // Index out of bounds triggers panic
}
```

Rust prevents buffer overreads by panicking on invalid array access, unlike C where this causes undefined behavior.

### Debugging with Backtraces

Set `RUST_BACKTRACE=1` to get a stack trace:

```bash
$ RUST_BACKTRACE=1 cargo run
thread 'main' panicked at src/main.rs:4:6:
index out of bounds: the len is 3 but the index is 99
stack backtrace:
   0: rust_begin_unwind
   1: core::panicking::panic_fmt
   2: core::panicking::panic_bounds_check
   3: <usize as core::slice::index::SliceIndex<[T]>>::index
   4: core::slice::index::<impl core::ops::index::Index<I> for [T]>::index
   5: <alloc::vec::Vec<T,A> as core::ops::index::Index<I>>::index
   6: panic::main  // <-- Your code here
             at ./src/main.rs:4:6
   7: core::ops::function::FnOnce::call_once
```

Start debugging from the first line mentioning your code (line 6 above). Lines above are library code that called your code; lines below are code that called your function.

### When to Use Panic

**Use `panic!` for:**
- Programming errors (bugs in your logic)
- Violated invariants that should never happen
- Security vulnerabilities from invalid data
- Contract violations in functions

**Example:**
```rust
fn get_user(id: u32) -> User {
    if id == 0 {
        panic!("User ID cannot be zero"); // Contract violation
    }
    // ... rest of function
}
```

For recoverable errors (network failures, file not found), use `Result<T, E>` instead.

[to-panic-or-not-to-panic]: ch09-03-to-panic-or-not-to-panic.html#to-panic-or-not-to-panic
