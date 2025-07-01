## Using Message Passing to Transfer Data Between Threads

Message passing enables thread communication by sending data through channels. Rust implements channels via `std::sync::mpsc` (multiple producer, single consumer).

### Channel Basics

Channels provide a transmitter/receiver pair for unidirectional data flow:

<Listing number="16-6" file-name="src/main.rs" caption="Creating a channel and assigning the two halves to `tx` and `rx`">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-06/src/main.rs}}
```

</Listing>

`mpsc::channel()` returns `(Sender<T>, Receiver<T>)`. The `mpsc` design allows multiple senders but only one receiver.

### Sending Data Between Threads

<Listing number="16-7" file-name="src/main.rs" caption='Moving `tx` to a spawned thread and sending `"hi"`'>

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-07/src/main.rs}}
```

</Listing>

The `send` method returns `Result<(), SendError<T>>`. It fails when the receiver is dropped.

<Listing number="16-8" file-name="src/main.rs" caption='Receiving the value `"hi"` in the main thread and printing it'>

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-08/src/main.rs}}
```

</Listing>

Receiver methods:
- `recv()`: Blocks until a value arrives, returns `Result<T, RecvError>`
- `try_recv()`: Non-blocking, returns `Result<T, TryRecvError>` immediately

### Channels and Ownership

Channels transfer ownership of sent values, preventing use-after-send errors:

<Listing number="16-9" file-name="src/main.rs" caption="Attempting to use `val` after we've sent it down the channel">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-09/src/main.rs}}
```

</Listing>

Compilation fails with:

```console
{{#include ../listings/ch16-fearless-concurrency/listing-16-09/output.txt}}
```

The `send` operation moves ownership to the receiver, preventing data races.

### Multiple Values and Iterator Pattern

<Listing number="16-10" file-name="src/main.rs" caption="Sending multiple messages and pausing between each one">

```rust,noplayground
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-10/src/main.rs}}
```

</Listing>

The receiver implements `Iterator`, terminating when the channel closes (all senders dropped).

### Multiple Producers

Clone the transmitter to create multiple senders:

<Listing number="16-11" file-name="src/main.rs" caption="Sending multiple messages from multiple producers">

```rust,noplayground
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-11/src/main.rs:here}}
```

</Listing>

Each cloned sender can send independently. Message order depends on thread scheduling and is non-deterministic.
