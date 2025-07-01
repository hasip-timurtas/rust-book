## Error Output to stderr

Proper CLI tools distinguish between regular output (stdout) and error messages (stderr) for better shell integration.

### Current Problem

Our error messages currently go to stdout:

```console
$ cargo run > output.txt
Problem parsing arguments: not enough arguments
```

The error appears in `output.txt` instead of the terminal, making debugging difficult when output is redirected.

### Solution: `eprintln!` Macro

<Listing number="12-24" file-name="src/main.rs" caption="Using `eprintln!` for error output">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-24/src/main.rs:here}}
```

</Listing>

### Testing the Fix

Error case with redirection:
```console
$ cargo run > output.txt
Problem parsing arguments: not enough arguments
```

Now the error appears on terminal while `output.txt` remains empty.

Success case with redirection:
```console
$ cargo run -- to poem.txt > output.txt
```

Terminal shows nothing, `output.txt` contains results:
```text
Are you nobody, too?
How dreary to be somebody!
```

## Summary

This implementation demonstrates:
- **Code organization**: Clean separation between main.rs and lib.rs
- **Error handling**: Proper Result types and error propagation
- **Testing**: TDD approach with isolated, testable functions
- **CLI patterns**: Command line args, environment variables, proper output streams
- **Rust features**: Ownership, lifetimes, traits, and error handling

The resulting CLI tool follows Unix conventions and demonstrates production-ready patterns for Rust applications.
