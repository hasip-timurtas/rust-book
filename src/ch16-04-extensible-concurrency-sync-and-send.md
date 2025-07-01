## Extensible Concurrency with the `Send` and `Sync` Traits

<!-- Old link, do not remove -->

<a id="extensible-concurrency-with-the-sync-and-send-traits"></a>

Rust's concurrency safety is enforced through two marker traits: `Send` and `Sync`. These traits are implemented automatically for most types but can be manually implemented for custom concurrent types.

### The `Send` Trait

`Send` indicates that ownership of a type can be transferred between threads safely. Almost all Rust types implement `Send`, with notable exceptions:

- `Rc<T>`: Not `Send` because non-atomic reference counting creates race conditions
- Raw pointers: Lack safety guarantees

Types composed entirely of `Send` types automatically implement `Send`. When we attempted to send `Rc<Mutex<i32>>` between threads in Listing 16-14, the compiler prevented this due to `Rc<T>`'s lack of `Send` implementation.

### The `Sync` Trait  

`Sync` indicates that a type is safe to access from multiple threads simultaneously. A type `T` implements `Sync` if `&T` (an immutable reference to `T`) implements `Send`.

Types that don't implement `Sync`:
- `Rc<T>`: Non-atomic reference counting unsafe for concurrent access
- `RefCell<T>` and `Cell<T>`: Runtime borrow checking not thread-safe
- `Mutex<T>`: Implements `Sync` and enables multi-threaded shared access

Primitive types implement both `Send` and `Sync`. Types composed entirely of `Send` and `Sync` types automatically implement these traits.

### Manual Implementation Safety

Manually implementing `Send` and `Sync` requires `unsafe` code and careful consideration of concurrency invariants. Building new concurrent types outside the `Send`/`Sync` ecosystem demands deep understanding of thread safety guarantees.

For production systems, prefer established concurrent data structures from crates like `crossbeam` or `tokio` rather than implementing custom concurrent types.

## Summary

Rust's concurrency model combines:
- **Channels**: Message passing with ownership transfer
- **Mutexes**: Shared state with exclusive access
- **Atomic types**: Lock-free concurrent operations  
- **Send/Sync traits**: Compile-time thread safety guarantees

The type system and borrow checker eliminate data races and memory safety issues at compile time, enabling fearless concurrency. Concurrent programming becomes a design decision rather than a source of runtime bugs.

[sharing-a-mutext-between-multiple-threads]: ch16-03-shared-state.html#sharing-a-mutext-between-multiple-threads
[nomicon]: ../nomicon/index.html
