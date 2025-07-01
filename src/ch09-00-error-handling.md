# Error Handling

Software'de error'lar kaçınılmazdır. Rust error handling için kapsamlı özellikler sunar. Çoğu durumda error olasılığını acknowledge etmenizi ve action almanızı zorunlu kılar - bu robustness sağlar.

Rust error'ları iki kategoriye ayırır:

## Recoverable vs Unrecoverable Errors

**Recoverable**: _file not found_ gibi - kullanıcıya rapor et, retry operation
**Unrecoverable**: Array bounds overflow gibi bug'lar - program'ı durdur  

Çoğu dilde exception mechanism ikisini de aynı şekilde handle eder. Rust exception'ı yoktur:

- **`Result<T, E>`**: Recoverable error'lar için
- **`panic!` macro**: Unrecoverable error'lar için execution'ı durdurur

Bu chapter önce `panic!` sonra `Result<T, E>` return etmeyi kapsar. Error'dan recover mi edilmeli yoksa execution durdurulmalı mı kararına odaklanır.
