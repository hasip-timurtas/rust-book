## Macros

Compile-time metaprogramming through declarative and procedural macros.

### Macros vs Functions

**Macros**: Variable parameters, operate on tokens, generate code at compile-time
**Functions**: Fixed parameters, runtime execution, typed arguments

### Declarative Macros (`macro_rules!`)

Pattern match on code structure:

```rust
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

**Pattern syntax**:
- `$x:expr` captures expressions
- `$( ... ),*` captures repeated patterns with `,` separator
- `$()*` repeats code for each match

### Procedural Macros

Transform `TokenStream` → `TokenStream`. Three types:

#### 1. Custom `derive` Macros

```rust
#[derive(HelloMacro)]
struct Pancakes;

fn main() {
    Pancakes::hello_macro();
}
```

**Implementation**:
```rust
#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();
    impl_hello_macro(&ast)
}
```

#### 2. Attribute-like Macros

```rust
#[route(GET, "/")]
fn index() {}
```

**Signature**:
```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream
```

#### 3. Function-like Macros

```rust
let sql = sql!(SELECT * FROM posts WHERE id=1);
```

**Signature**:
```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream
```

### Key Crates

- **`syn`**: Parse Rust code into AST
- **`quote`**: Generate Rust code from templates  
- **`proc_macro`**: Compiler API for token manipulation

### Practical Applications

**Declarative**: Domain-specific syntax, boilerplate reduction, type-safe builders
**Procedural**: ORM derive macros, framework attributes, compile-time validation

**Performance**: All expansion happens at compile-time with zero runtime cost.

[ref]: ../reference/macros-by-example.html
[tlborm]: https://veykril.github.io/tlborm/
[`syn`]: https://crates.io/crates/syn
[`quote`]: https://crates.io/crates/quote
[syn-docs]: https://docs.rs/syn/2.0/syn/struct.DeriveInput.html
[quote-docs]: https://docs.rs/quote
[decl]: #declarative-macros-with-macro_rules-for-general-metaprogramming
