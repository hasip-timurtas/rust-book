# Rust Kitabı - Senior Geliştiriciler İçin Yeniden Yazıldı

Bu repo, The Rust Programming Language kitabının deneyimli TypeScript/JavaScript/Node.js geliştiriciler için yeniden yazılan versiyonunu içerir.

## 🎯 Hedef Kitle

- Senior full stack developers
- TypeScript, JavaScript, Node.js deneyimi
- Rust'ta entry-level
- Sistem tasarımı, performans, clean code, güvenli mimari bilgisi

## ✨ Yaklaşım

### ✅ Yapılan Değişiklikler
- **Türkçe'ye çeviri** - Teknik İngilizce terminoloji korunarak
- **Concise & technical** - Gereksiz açıklamalar kaldırıldı  
- **Beginner tone removal** - Deneyimli developer bakış açısı
- **TypeScript/Node.js comparisons** - Sadece gerçekten faydalı olduğunda
- **Practical & minimal** - Doğrudan kullanılabilir örnekler

### ❌ Kaldırılanlar
- Uzun analogies ve metaphors
- Step-by-step beginner guidance
- Tekrarlayan açıklamalar
- Basic programming concepts

## 📚 Tamamlanan Bölümler

### Temel Kavramlar
- ✅ **Önsöz** - Rust'ın value proposition'ı, senior dev perspektifi
- ✅ **Cargo** - Build system, npm ile karşılaştırmalı
- ✅ **Ownership Sistemi** - Memory management fundamentals

### Memory Management
- ✅ **Ownership** - Move semantics, TypeScript referanslarla karşılaştırma
- ✅ **References & Borrowing** - Borrow checker, data races prevention
- ✅ **Slices** - Zero-copy views, JavaScript array.slice() karşılaştırması

### Data Structures  
- ✅ **Structs** - TypeScript interfaces ile karşılaştırma, ownership patterns
- ✅ **Enums** - Algebraic data types, TypeScript union types karşılaştırması

## 🔧 Teknik Vurgular

### Memory Safety
```rust
// TypeScript: Runtime risk
let arr = [1, 2, 3];
let ref1 = arr;
let ref2 = arr;
ref1.push(4); // ref2 also affected

// Rust: Compile-time safety
let mut arr = vec![1, 2, 3];
let ref1 = &mut arr;
let ref2 = &mut arr; // ❌ Compile error
```

### Zero-Cost Abstractions
```rust
// Struct definition - compile-time layout optimization
struct User {
    username: String,  // Owned data
    email: String,     // Zero runtime cost for abstractions
}
```

### Type Safety
```rust
// Option<T> - Null safety guarantee
fn get_length(s: Option<String>) -> usize {
    match s {
        Some(string) => string.len(),
        None => 0,  // ✅ Must handle None case
    }
}
```

## 🚀 Öne Çıkan Özellikler

### 1. **Practical Code Examples**
- Direkt kullanılabilir kod parçacıkları
- Real-world scenarios
- Production-ready patterns

### 2. **Architecture Insights**
- Memory layout optimizations  
- Compile-time vs runtime trade-offs
- System design implications

### 3. **Developer Experience**
- Cargo workflow (npm equivalent)
- Error handling patterns
- Debugging approaches

## 📈 Öğrenme Yolu

1. **Ownership** - Rust'ın temel differentiator'ı
2. **Borrowing** - Memory safety without GC
3. **Data Structures** - Zero-cost abstractions
4. **Pattern Matching** - Type-safe control flow
5. **Error Handling** - `Result<T, E>` ve `Option<T>`

## 🔗 TypeScript Geliştiriciler İçin

### Benzerlikler
- Type safety ve static analysis
- Compile-time error catching
- Generic programming
- Pattern matching (TS'de discriminated unions)

### Farklar  
- Manual memory management (ownership)
- Zero-cost abstractions
- Immutability by default
- Explicit error handling

## 🎓 Sonraki Adımlar

Tamamlanan temel kavramlarla:
- Concurrent programming (async/await)
- Error handling (`Result<T, E>`)
- Generic types ve traits
- Advanced ownership patterns
- Performance optimizations

Bu yaklaşım, senior geliştiricilerin Rust'ı hızlı ve etkili şekilde öğrenmelerini sağlarken, sistem programlama becerilerini geliştirmelerine odaklanır.

# The Rust Programming Language

![Build Status](https://github.com/rust-lang/book/workflows/CI/badge.svg)

This repository contains the source of "The Rust Programming Language" book.

[The book is available in dead-tree form from No Starch Press][nostarch].

[nostarch]: https://nostarch.com/rust-programming-language-2nd-edition

You can also read the book for free online. Please see the book as shipped with
the latest [stable], [beta], or [nightly] Rust releases. Be aware that issues
in those versions may have been fixed in this repository already, as those
releases are updated less frequently.

[stable]: https://doc.rust-lang.org/stable/book/
[beta]: https://doc.rust-lang.org/beta/book/
[nightly]: https://doc.rust-lang.org/nightly/book/

See the [releases] to download just the code of all the code listings that appear in the book.

[releases]: https://github.com/rust-lang/book/releases

## Requirements

Building the book requires [mdBook], ideally the same version that
rust-lang/rust uses in [this file][rust-mdbook]. To get it:

[mdBook]: https://github.com/rust-lang/mdBook
[rust-mdbook]: https://github.com/rust-lang/rust/blob/master/src/tools/rustbook/Cargo.toml

```bash
$ cargo install mdbook --locked --version <version_num>
```

The book also uses two mdbook plugins which are part of this repository. If you
do not install them, you will see warnings when building and the output will not
look right, but you _will_ still be able to build the book. To use the plugins,
you should run:

```bash
$ cargo install --locked --path packages/mdbook-trpl
```

## Building

To build the book, type:

```bash
$ mdbook build
```

The output will be in the `book` subdirectory. To check it out, open it in
your web browser.

_Firefox:_

```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_

```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

To run the tests:

```bash
$ cd packages/trpl
$ mdbook test --library-path packages/trpl/target/debug/deps
```

## Contributing

We'd love your help! Please see [CONTRIBUTING.md][contrib] to learn about the
kinds of contributions we're looking for.

[contrib]: https://github.com/rust-lang/book/blob/main/CONTRIBUTING.md

Because the book is [printed][nostarch], and because we want
to keep the online version of the book close to the print version when
possible, it may take longer than you're used to for us to address your issue
or pull request.

So far, we've been doing a larger revision to coincide with [Rust Editions](https://doc.rust-lang.org/edition-guide/). Between those larger
revisions, we will only be correcting errors. If your issue or pull request
isn't strictly fixing an error, it might sit until the next time that we're
working on a large revision: expect on the order of months or years. Thank you
for your patience!

### Translations

We'd love help translating the book! See the [Translations] label to join in
efforts that are currently in progress. Open a new issue to start working on
a new language! We're waiting on [mdbook support] for multiple languages
before we merge any in, but feel free to start!

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook support]: https://github.com/rust-lang/mdBook/issues/5

## Spellchecking

To scan source files for spelling errors, you can use the `spellcheck.sh`
script available in the `ci` directory. It needs a dictionary of valid words,
which is provided in `ci/dictionary.txt`. If the script produces a false
positive (say, you used the word `BTreeMap` which the script considers invalid),
you need to add this word to `ci/dictionary.txt` (keep the sorted order for
consistency).
