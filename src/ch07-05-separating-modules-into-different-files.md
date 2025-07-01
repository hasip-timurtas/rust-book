## Separating Modules into Different Files

As modules grow, separate them into individual files for better organization. Unlike JavaScript/TypeScript where files are implicit modules, Rust requires explicit module declarations.

### Moving Modules to Files

Starting from inline modules, extract them to separate files:

<Listing number="7-21" file-name="src/lib.rs" caption="Declaring the `front_of_house` module whose body will be in *src/front_of_house.rs*">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/lib.rs}}
```

</Listing>

<Listing number="7-22" file-name="src/front_of_house.rs" caption="Definitions inside the `front_of_house` module in *src/front_of_house.rs*">

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/front_of_house.rs}}
```

</Listing>

The `mod` declaration loads the file once—it's not an "include" operation. Other files reference the loaded module via its declared path.

### Extracting Submodules

For submodules, create a directory structure:

<Listing file-name="src/front_of_house.rs">

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house.rs}}
```

</Listing>

<Listing file-name="src/front_of_house/hosting.rs">

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house/hosting.rs}}
```

</Listing>

The file location must match the module hierarchy—`hosting` goes in `src/front_of_house/hosting.rs` because it's a child of `front_of_house`.

### File Path Conventions

Rust supports two file organization styles:

**Modern style** (recommended):
- `src/front_of_house.rs`
- `src/front_of_house/hosting.rs`

**Legacy style** (still supported):
- `src/front_of_house/mod.rs`
- `src/front_of_house/hosting/mod.rs`

Don't mix styles within the same project to avoid confusion.

## Summary

Rust's module system provides explicit control over code organization and visibility:

- **Packages** contain crates with `Cargo.toml` manifests
- **Crates** are compilation units (binary or library)
- **Modules** organize code with privacy controls (private by default)
- **Paths** address items using `::` syntax (absolute with `crate::` or relative)
- **`use`** creates local shortcuts to reduce path repetition
- **`pub`** exposes items across module boundaries

This system enforces encapsulation at compile time, unlike JavaScript's runtime-based privacy patterns.

[paths]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
