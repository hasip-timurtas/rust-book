## Cargo

Cargo, Rust'ın build sistemi ve paket yöneticisidir. npm'in Node.js ekosistemindeki rolüne benzer şekilde dependency management, build süreçleri ve proje yapılandırmasını handle eder.

### Proje Oluşturma

```console
$ cargo new hello_cargo
$ cd hello_cargo
```

Cargo şu yapıyı oluşturur:
- `Cargo.toml`: Proje metadata ve dependencies (package.json benzeri)
- `src/main.rs`: Entry point
- Git repository (varsa existing repo'da override edilmez)

### Cargo.toml

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2024"

[dependencies]
```

TOML formatında configuration. `edition` field'ı language features'ları belirler.

### Build ve Run

```console
$ cargo build          # target/debug/'da executable oluşturur
$ cargo run            # build + run tek komutta
$ cargo check          # compilation check, executable oluşturmaz (hızlı)
$ cargo build --release # target/release/'da optimized build
```

`cargo check` development workflow'unda type checking için ideal - executable üretmediği için çok hızlı.

### Development vs Release

- **Debug build**: Hızlı compile, optimizasyon yok, debug symbols var
- **Release build**: Yavaş compile, aggressive optimizations, production-ready

### Workspace Konsepti

Cargo monorepo'ları destekler:

```console
$ git clone example.org/project
$ cd project
$ cargo build
```

Existing project'lerde standart workflow.

## Özet

Cargo temel komutları:
- `cargo new`: Proje oluştur
- `cargo build/run`: Development build
- `cargo check`: Type checking (hızlı)
- `cargo build --release`: Production build

Cargo npm'e benzer şekilde dependency management ve build orchestration sağlar, ancak sistem seviyesi optimizasyonlarla.
