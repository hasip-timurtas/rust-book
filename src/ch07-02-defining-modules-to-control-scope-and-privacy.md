## Defining Modules to Control Scope and Privacy

### Module System Quick Reference

- **Crate root**: Compiler starts at `src/lib.rs` (library) or `src/main.rs` (binary)
- **Module declaration**: `mod garden;` looks for code in:
  - Inline: `mod garden { ... }`
  - File: `src/garden.rs`
  - Directory: `src/garden/mod.rs`
- **Submodules**: Declared in parent module files, found in subdirectories
- **Paths**: Access items via `crate::module::item` (absolute) or `module::item` (relative)
- **Privacy**: Items are private by default; use `pub` to expose them
- **`use` keyword**: Creates shortcuts to reduce path repetition

### Example Structure

```text
backyard
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

<Listing number="07-3" file-name="src/main.rs" caption="Example code in src/main.rs">

```rust,editable,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/main.rs}}
```

</Listing>

<Listing number="07-4" file-name="src/garden.rs" caption="Example code in src/garden.rs">

```rust,editable,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/garden.rs}}
```

</Listing>

<Listing number="07-5" file-name="src/garden/vegetables.rs" caption="Example code in src/garden/vegetables.rs">

```rust,editable,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/garden/vegetables.rs}}
```

</Listing>

### Organizing Code with Modules

Modules provide namespace organization and privacy control. Unlike JavaScript imports which expose everything explicitly exported, Rust items are private by default.

<Listing number="7-1" file-name="src/lib.rs" caption="A `front_of_house` module containing other modules that then contain functions">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-01/src/lib.rs}}
```

</Listing>

This creates a module tree:

<Listing number="7-2" caption="The module tree for the code in Listing 7-1">

```text
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

</Listing>

The crate root forms an implicit `crate` module. Child modules can access parent items, but parents cannot access private child items without explicit `pub` declarations.
