## Environment Variables

Add case-insensitive search controlled by environment variables.

### Test for Case-Insensitive Search

<Listing number="12-20" file-name="src/lib.rs" caption="Adding test for case-insensitive functionality">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-20/src/lib.rs:here}}
```

</Listing>

### Implementation

<Listing number="12-21" file-name="src/lib.rs" caption="Case-insensitive search function">

```rust,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-21/src/lib.rs:here}}
```

</Listing>

Key points:
- `to_lowercase()` creates new `String` data, changing the type from `&str` to `String`
- Need `&` when passing to `contains()` since it expects `&str`
- This handles basic Unicode but isn't 100% accurate for all cases

### Configuration Integration

Add environment variable support to the `Config` struct:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-22/src/main.rs:here}}
```

Update the `run` function to use the appropriate search function:

<Listing number="12-22" file-name="src/main.rs" caption="Conditional search based on configuration">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-22/src/main.rs:there}}
```

</Listing>

### Environment Variable Detection

<Listing number="12-23" file-name="src/main.rs" caption="Checking for `IGNORE_CASE` environment variable">

```rust,ignore,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-23/src/main.rs:here}}
```

</Listing>

Usage patterns:
- `env::var()` returns `Result<String, VarError>`
- `is_ok()` returns `true` if variable is set (regardless of value)
- We only care about presence, not the actual value

### Testing

```console
$ cargo run -- to poem.txt
Are you nobody, too?
How dreary to be somebody!
```

```console
$ IGNORE_CASE=1 cargo run -- to poem.txt
Are you nobody, too?
How dreary to be somebody!
To tell your name the livelong day
To an admiring bog!
```

**PowerShell:**
```console
PS> $Env:IGNORE_CASE=1; cargo run -- to poem.txt
PS> Remove-Item Env:IGNORE_CASE  # To unset
```

This pattern allows flexible configuration without requiring command-line flags for every option.
