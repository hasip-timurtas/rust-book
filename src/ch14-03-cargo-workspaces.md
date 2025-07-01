## Cargo Workspaces

Workspaces manage multiple related crates with shared dependencies and a unified build system.

### Creating a Workspace

Create workspace structure:
```console
$ mkdir add
$ cd add
```

**Workspace `Cargo.toml`:**
```toml
{{#include ../listings/ch14-more-about-cargo/no-listing-01-workspace/add/Cargo.toml}}
```

Add binary crate:
```console
$ cargo new adder
     Created binary (application) `adder` package
      Adding `adder` as member of workspace at `file:///projects/add`
```

Workspace structure:
```text
├── Cargo.lock
├── Cargo.toml
├── adder
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── target
```

**Key points:**
- Single `target` directory for all workspace members
- Shared `Cargo.lock` ensures consistent dependency versions
- Build from workspace root with `cargo build`

### Adding Library Crates

```console
$ cargo new add_one --lib
     Created library `add_one` package
```

**Library implementation:**
```rust,editable,noplayground
// add_one/src/lib.rs
pub fn add_one(x: i32) -> i32 {
    x + 1
}
```

**Workspace dependencies:**
```toml
# adder/Cargo.toml
[dependencies]
add_one = { path = "../add_one" }
```

**Using workspace dependencies:**
<Listing number="14-7" file-name="adder/src/main.rs" caption="Using library from workspace">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch14-more-about-cargo/listing-14-07/add/adder/src/main.rs}}
```

</Listing>

### Workspace Commands

**Build workspace:**
```console
$ cargo build
   Compiling add_one v0.1.0 (file:///projects/add/add_one)
   Compiling adder v0.1.0 (file:///projects/add/adder)
```

**Run specific package:**
```console
$ cargo run -p adder
     Running `target/debug/adder`
Hello, world! 10 plus one is 11!
```

**Test specific package:**
```console
$ cargo test -p add_one
```

### External Dependencies

Dependencies must be explicitly added to each crate's `Cargo.toml`:

```toml
# add_one/Cargo.toml
[dependencies]
rand = "0.8.5"
```

Workspace ensures all crates use the same version of shared dependencies.

### Testing

**Run all tests:**
```console
$ cargo test
```

**Run specific crate tests:**
```console
$ cargo test -p add_one
```

### Publishing

Each workspace member publishes independently:
```console
$ cargo publish -p add_one
$ cargo publish -p adder
```

### Benefits

- **Shared dependencies**: Consistent versions across all crates
- **Unified build**: Single command builds entire workspace  
- **Cross-crate development**: Easy local dependencies between related crates
- **Testing**: Run tests across all crates simultaneously
- **Code sharing**: Common utilities accessible to all workspace members

Workspaces are ideal for:
- Multi-crate applications with shared libraries
- Plugin architectures
- Related tools that share common functionality
