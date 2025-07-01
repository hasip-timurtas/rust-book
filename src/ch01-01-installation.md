## Installation

Install Rust via `rustup`, the official toolchain manager that handles Rust versions, targets, and associated tools.

### Install rustup

**Unix/macOS:**
```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

**Windows:**
Download and run the installer from [https://www.rust-lang.org/tools/install][install]

The installer includes:
- `rustc` (compiler)
- `cargo` (package manager/build system)
- `rustfmt` (code formatter)
- `clippy` (linter)
- Local documentation

### Dependencies

**macOS:**
```console
$ xcode-select --install
```

**Ubuntu/Debian:**
```console
$ sudo apt install build-essential
```

**Windows:**
Visual Studio Build Tools or Visual Studio with C++ development tools.

### Verification

```console
$ rustc --version
$ cargo --version
```

### Management Commands

```console
$ rustup update              # Update to latest stable
$ rustup self uninstall      # Remove rustup and Rust
$ rustup doc                 # Open local documentation
```

### IDE Setup

Recommended: VS Code with `rust-analyzer` extension (provides LSP features comparable to TypeScript's language server).

### Offline Development

For projects requiring external dependencies:
```console
$ cargo new get-dependencies
$ cd get-dependencies
$ cargo add rand@0.8.5 trpl@0.2.0
```

Use `--offline` flag with cargo commands to use cached dependencies.

[install]: https://www.rust-lang.org/tools/install
