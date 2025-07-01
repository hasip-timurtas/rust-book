# Fearless Concurrency

Concurrency programming - programın farklı kısımlarının independent olarak execute olması - her zaman zor olmuştur. Node.js single-threaded event loop model kullanırken, Rust native multi-threading sunar.

## Concurrent Programming Zorlukları

Geleneksel olarak:
- **Race conditions**: Thread'ler data'ya inconsistent order'da erişir
- **Deadlock'lar**: Thread'ler birbirini bekler
- **Debug zor bug'lar**: Sadece specific condition'larda reproduce olur

Rust ownership ve type system ile bu problemleri **compile-time**'da yakalar - "fearless concurrency".

## Bu Chapter'ın Kapsamı

- **Thread'ler**: Kod parçalarını simultaneously çalıştırma
- **Message passing**: Channel'lar ile thread'ler arası communication
- **Shared state**: Mutex ve Arc ile data sharing
- **Sync ve Send traits**: Concurrency guarantee'leri extend etme

Rust approach: Ownership rules concurrency bug'larını compile-time'da yakalar. Runtime overhead minimal.
