## File Reading

Add file reading capability to complete the basic functionality:

<Listing number="12-3" file-name="poem.txt" caption="Test file with sample content">

```text
{{#include ../listings/ch12-an-io-project/listing-12-03/poem.txt}}
```

</Listing>

<Listing number="12-4" file-name="src/main.rs" caption="Reading file contents">

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-04/src/main.rs:here}}
```

</Listing>

Key points:
- `fs::read_to_string()` returns `Result<String, std::io::Error>`
- Currently using `expect()` for error handling (will improve in next section)
- The `main` function is handling multiple responsibilities (argument parsing, file reading)

Current issues to address:
1. Poor separation of concerns
2. Inadequate error handling
3. No validation of input arguments

The next section addresses these through refactoring.
