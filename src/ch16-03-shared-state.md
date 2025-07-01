## Shared-State Concurrency

Unlike message passing's single ownership model, shared-state concurrency allows multiple threads to access the same memory locations simultaneously. Rust provides thread-safe primitives through mutexes and atomic reference counting.

### Mutexes for Exclusive Data Access

A mutex (_mutual exclusion_) guards data with a locking mechanism, ensuring only one thread accesses the data at a time.

<Listing number="16-12" file-name="src/main.rs" caption="Exploring the API of `Mutex<T>` in a single-threaded context for simplicity">

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-12/src/main.rs}}
```

</Listing>

Key `Mutex<T>` characteristics:
- `lock()` returns `LockResult<MutexGuard<T>>` 
- `MutexGuard<T>` implements `Deref` to access inner data
- `Drop` implementation automatically releases the lock when guard goes out of scope
- Lock acquisition blocks until available or panics if poisoned

### Sharing Mutexes Between Threads

Attempting to share a mutex across threads fails without proper ownership handling:

<Listing number="16-13" file-name="src/main.rs" caption="Ten threads, each incrementing a counter guarded by a `Mutex<T>`">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-13/src/main.rs}}
```

</Listing>

Error: cannot move `counter` into multiple threads:

```console
{{#include ../listings/ch16-fearless-concurrency/listing-16-13/output.txt}}
```

### Multiple Ownership with `Rc<T>` Limitation

`Rc<T>` provides multiple ownership but is not thread-safe:

<Listing number="16-14" file-name="src/main.rs" caption="Attempting to use `Rc<T>` to allow multiple threads to own the `Mutex<T>`">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-14/src/main.rs}}
```

</Listing>

Error: `Rc<T>` lacks `Send` trait for thread safety:

```console
{{#include ../listings/ch16-fearless-concurrency/listing-16-14/output.txt}}
```

`Rc<T>` uses non-atomic reference counting, making it unsafe for concurrent access.

### Atomic Reference Counting with `Arc<T>`

`Arc<T>` (_atomically reference-counted_) provides thread-safe multiple ownership:

<Listing number="16-15" file-name="src/main.rs" caption="Using an `Arc<T>` to wrap the `Mutex<T>` to be able to share ownership across multiple threads">

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-15/src/main.rs}}
```

</Listing>

`Arc<T>` uses atomic operations for reference counting, ensuring thread safety. Atomic operations have performance overhead, so use only when needed for concurrent access.

### Interior Mutability Pattern

`Mutex<T>` provides interior mutability similar to `RefCell<T>`:
- `RefCell<T>`/`Rc<T>`: Single-threaded shared ownership with runtime borrow checking
- `Mutex<T>`/`Arc<T>`: Multi-threaded shared ownership with locking

Both patterns allow mutation through immutable references, but `Mutex<T>` includes deadlock risks when acquiring multiple locks.
