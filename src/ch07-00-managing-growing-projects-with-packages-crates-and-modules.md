# Managing Growing Projects with Packages, Crates, and Modules

Rust's module system provides hierarchical code organization with explicit visibility control, similar to TypeScript's module system but with compile-time enforcement.

## Module System Components

- **Packages**: Cargo's build unit (like npm packages)
- **Crates**: Compilation units that produce libraries or executables  
- **Modules**: Namespace and privacy boundaries within crates
- **Paths**: Item addressing system (structs, functions, modules)

## Key Concepts

**Scope and Privacy**: Unlike JavaScript's function-scoped variables, Rust uses explicit privacy controls (`pub`) with module-based scoping.

**Compared to TypeScript**: Similar import/export concepts but with stronger encapsulation guarantees and zero-runtime-cost abstraction.

This chapter covers organizing large codebases through strategic module design and dependency management.

[workspaces]: ch14-03-cargo-workspaces.html
