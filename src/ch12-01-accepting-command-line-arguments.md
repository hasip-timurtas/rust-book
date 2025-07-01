## Command Line Arguments

Create the project and handle command line arguments:

```console
$ cargo new minigrep
$ cd minigrep
```

Target usage: `cargo run -- searchstring example-filename.txt`

### Reading Arguments

Use `std::env::args` to access command line arguments:

<Listing number="12-1" file-name="src/main.rs" caption="Collecting command line arguments">

```rust
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-01/src/main.rs}}
```

</Listing>

Key points:
- `env::args()` returns an iterator over arguments
- `collect()` converts the iterator to `Vec<String>`
- First argument (`args[0]`) is always the binary name
- Invalid Unicode in arguments will panic; use `env::args_os()` for `OsString` if needed

### Storing Arguments

Extract the required arguments into variables:

<Listing number="12-2" file-name="src/main.rs" caption="Extracting query and file path arguments">

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-02/src/main.rs}}
```

</Listing>

This basic implementation lacks error handling for insufficient arguments - we'll address that in the refactoring section.

[ch13]: ch13-00-functional-features.html
[ch7-idiomatic-use]: ch07-04-bringing-paths-into-scope-with-the-use-keyword.html#creating-idiomatic-use-paths
