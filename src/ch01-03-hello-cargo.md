## Hello, Cargo!

Cargo is Rust's integrated build system and package manager—equivalent to npm/yarn but with built-in compilation, testing, and project management.

### Project Creation

```console
$ cargo new hello_cargo
$ cd hello_cargo
```

**Generated structure:**
```
hello_cargo/
├── Cargo.toml          # Package manifest (like package.json)
├── .gitignore          # Git ignore file
└── src/
    └── main.rs         # Source code
```

### Cargo.toml

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2024"        # Rust edition (language version)

[dependencies]          # Dependencies (like package.json dependencies)
```

**vs package.json:**
- TOML format instead of JSON
- `edition` field specifies Rust language features
- Dependencies are automatically compiled, not interpreted

### Essential Commands

```console
$ cargo build           # Compile (creates target/debug/)
$ cargo run             # Build + execute
$ cargo check           # Type check without generating binary
$ cargo build --release # Optimized build (target/release/)
$ cargo test            # Run tests
$ cargo doc --open      # Generate and open documentation
```

### Development Workflow

**Debug builds** (default):
- Fast compilation
- Includes debugging symbols
- No optimizations
- Located in `target/debug/`

**Release builds:**
- Slower compilation
- Full optimizations
- Smaller binaries
- Located in `target/release/`

### Key Differences from npm/yarn

| Feature | npm/yarn | Cargo |
|---------|----------|-------|
| **Installation** | Runtime dependency resolution | Compile-time linking |
| **Execution** | Requires Node.js runtime | Standalone binary |
| **Versioning** | Semantic versioning | Semantic versioning + edition system |
| **Lock file** | package-lock.json/yarn.lock | Cargo.lock |
| **Scripts** | package.json scripts | Built-in commands |

### Cargo.lock

Automatically generated dependency lock file (like package-lock.json). Ensures reproducible builds across environments. Commit to version control for applications, not for libraries.

### Integration with Existing Projects

```console
$ git clone <repo>
$ cd <project>
$ cargo build          # Downloads deps, compiles everything
```

No separate dependency installation step—Cargo handles everything in the build process.

Cargo becomes essential for:
- Dependency management
- Multi-file projects
- Testing and documentation
- Publishing to crates.io (Rust's package registry)
