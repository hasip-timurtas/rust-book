# Managing Growing Projects with Packages, Crates, and Modules

Rust's module system provides tools for organizing code, controlling scope, and managing visibility—essential for building maintainable applications. Unlike JavaScript/TypeScript's file-based modules, Rust uses an explicit module hierarchy with compile-time visibility controls.

## Core Concepts

Rust's module system consists of:

- **Packages**: Cargo's build units containing one or more crates
- **Crates**: Compilation units producing libraries or executables  
- **Modules**: Organizational units controlling scope and privacy
- **Paths**: Item addressing within the module tree

## Key Differences from JavaScript/TypeScript

Unlike ES modules where each file is implicitly a module, Rust modules are explicitly declared and can span multiple files or exist inline. Privacy is explicit—items are private by default, unlike JavaScript's implicit public exports.

The module system enforces encapsulation at compile time, preventing access to private implementation details that could be accessed through reflection or direct property access in JavaScript.

[workspaces]: ch14-03-cargo-workspaces.html
