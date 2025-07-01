## Hello, World!

### Direct Compilation

Create `main.rs`:

```rust,editable
fn main() {
    println!("Hello, world!");
}
```

Compile and run:
```console
$ rustc main.rs
$ ./main                    # Linux/macOS
$ .\main.exe               # Windows
```

### Program Structure

```rust,editable
fn main() {                 // Entry point (like main() in C)
    println!("Hello, world!");  // Macro call (note the !)
}
```

**Key differences from TypeScript/Node.js:**
- **Ahead-of-time compilation**: Unlike Node.js's JIT, Rust compiles to native binaries
- **Macros**: `println!` is a macro (indicated by `!`), not a function
- **No runtime**: Unlike Node.js, compiled binaries run without a runtime environment

### Compilation Model

Unlike interpreted languages (JavaScript) or VM languages with JIT (Java), Rust uses ahead-of-time compilation:

1. **Source** → **Binary**: Direct compilation to machine code
2. **Zero dependencies**: Binaries run without Rust installation
3. **Static linking**: All dependencies bundled in the executable

This is similar to Go's compilation model but with stronger memory safety guarantees.

For larger projects, use Cargo instead of direct `rustc` compilation.
