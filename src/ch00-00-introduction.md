# Giriş

> Not: Bu kitabın versiyonu [No Starch Press][nsp] tarafından basılı ve e-kitap olarak yayınlanan [The Rust Programming Language][nsprust] ile aynıdır.

[nsprust]: https://nostarch.com/rust-programming-language-2nd-edition
[nsp]: https://nostarch.com/

_The Rust Programming Language_'e hoş geldiniz. Rust, daha hızlı ve güvenilir yazılım geliştirme sunar. High-level ergonomi ve low-level kontrolün dengelenmesi çoğu dilde zordur; Rust bu sorunu çözer.

## Rust Kimlere Hitap Eder

### Geliştirici Ekipleri

Rust, büyük ekiplerde işbirliği için verimli bir araçtır. Low-level kod subtil bug'lara eğilimlidir - bunları yakalamak için extensive testing ve deneyimli code review gerekir. Rust compiler gatekeeper rolü oynar ve bu bug'larla (concurrency bug'ları dahil) derleme yapmaz.

Modern developer tooling:
- **Cargo**: Dependency management ve build tool
- **Rustfmt**: Consistent code style  
- **rust-analyzer**: IDE integration, autocompletion, inline errors

### Sistem Kavramları Öğrenenler

Rust, sistem programlama konseptlerini öğrenmek isteyen developer'lar için idealdir.

### Şirketler  

Büyük küçük yüzlerce şirket production'da Rust kullanır: CLI tools, web services, DevOps tooling, embedded devices, media processing, blockchain, bioinformatics, search engines, IoT, ML ve Firefox browser.

### Hız ve Kararlılık Arayanlar

Rust zero-cost abstraction'lar sunar - high-level özellikler manual kod kadar hızlı compile olur. Güvenlik _ve_ performans, safety _ve_ productivity.

## Bu Kitap Kimler İçin

Bu kitap başka programlama dillerinde deneyimi olan senior developer'ları hedefler. TypeScript/Node.js background varsayar.

## Kitabı Nasıl Kullanmalı  

Sequentially okumayı varsayar. İki tip chapter:
- **Concept chapters**: Rust aspectleri
- **Project chapters**: Uygulamalı projeler (2, 12, 21)

### Chapter Özeti

**1-4**: Kurulum, Hello World, Cargo, temel konseptler, ownership  
**5-6**: Struct'lar, enum'lar, pattern matching  
**7-9**: Module system, collections, error handling  
**10**: Generics, traits, lifetimes  
**11-12**: Testing, I/O project (grep implementation)  
**13-15**: Closures/iterators, Cargo deep-dive, smart pointers  
**16-17**: Concurrency, async programming  
**18-20**: OOP patterns, advanced patterns, advanced features  
**21**: Multithreaded web server project

### Error Message'ları

Rust compiler error'ları öğrenme sürecinin parçasıdır. Ferris sembolü:

| Ferris | Anlamı |
|--------|--------|
| <img src="img/ferris/does_not_compile.svg" class="ferris-explain" alt="Ferris with a question mark"/> | Kod derlenmez |
| <img src="img/ferris/panics.svg" class="ferris-explain" alt="Ferris throwing up their hands"/> | Kod panic atar |
| <img src="img/ferris/not_desired_behavior.svg" class="ferris-explain" alt="Ferris with one claw up, shrugging"/> | İstenmeyen davranış |

## Kaynak Kod

Kitabın source file'ları [GitHub][book]'da mevcuttur.

[book]: https://github.com/rust-lang/book/tree/main/src
