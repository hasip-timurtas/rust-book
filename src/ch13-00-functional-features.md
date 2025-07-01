# Functional Language Features: Iterators ve Closures

Rust tasarımı functional programming'den ilham almıştır. Function'ları value olarak kullanma: argument'larda pass etme, return etme, variable'lara assign etme.

Bu chapter functional programming tartışması yapmaz, ancak JavaScript/TypeScript'teki benzer özellikleri bildiğinizi varsayarak Rust'ın functional özelliklerini kapsar.

## İçerik

- **Closures**: Variable'da saklanabilen function-like construct (JS arrow function'ları gibi)
- **Iterators**: Element serilerini process etme (JS Array methods benzeri)  
- Chapter 12 I/O project'ini closures ve iterators ile geliştirme
- Performance analizi (spoiler: çok hızlılar)

Pattern matching ve enum'lar da functional style'dan etkilenmiştir. Closures ve iterators idiomatic, hızlı Rust kodu yazmanın temeldir.
