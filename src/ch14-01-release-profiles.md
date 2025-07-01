## Customizing Builds with Release Profiles

Release profiles control compiler optimization levels and debugging information. Cargo uses two main profiles:

- **`dev`**: Used by `cargo build` (fast compilation, debugging info)
- **`release`**: Used by `cargo build --release` (optimized, production-ready)

```console
$ cargo build
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
$ cargo build --release
    Finished `release` profile [optimized] target(s) in 0.32s
```

### Default Settings

```toml
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

`opt-level` controls optimization intensity (0-3):
- **0**: No optimization, fastest compilation
- **3**: Maximum optimization, slower compilation

### Custom Configuration

Override defaults in `Cargo.toml`:

```toml
[profile.dev]
opt-level = 1
```

This applies moderate optimization to development builds, trading compile time for runtime performance.

For complete configuration options, see [Cargo's profile documentation](https://doc.rust-lang.org/cargo/reference/profiles.html).
