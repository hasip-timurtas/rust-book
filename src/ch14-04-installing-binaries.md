<!-- Old link, do not remove -->

<a id="installing-binaries-from-cratesio-with-cargo-install"></a>

## Installing Binaries with `cargo install`

Install command-line tools from [crates.io](https://crates.io/) for local development use.

**Installation location**: `$HOME/.cargo/bin` (ensure it's in your `$PATH`)

**Requirements**: Crate must have binary targets (contains `src/main.rs` or specified binary)

### Example: Installing ripgrep

```console
$ cargo install ripgrep
    Updating crates.io index
  Downloaded ripgrep v14.1.1
  Installing ripgrep v14.1.1
   Compiling grep v0.3.2
    Finished `release` profile [optimized + debuginfo] target(s) in 6.73s
  Installing ~/.cargo/bin/rg
   Installed package `ripgrep v14.1.1` (executable `rg`)
```

The installed binary (`rg`) is immediately available if `~/.cargo/bin` is in your PATH.

### Common Use Cases

- Development tools (`cargo-watch`, `cargo-expand`)  
- Text processing utilities (`ripgrep`, `fd`, `bat`)
- System administration tools
- Custom CLI applications from the Rust ecosystem

**Note**: `cargo install` is for developer tools, not system-wide package management.
