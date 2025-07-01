## Concurrency with Async

Async provides different APIs and performance characteristics compared to threads, but many patterns are similar. Key differences:
- Tasks are lightweight compared to OS threads
- Fair vs unfair scheduling
- Cooperative vs preemptive multitasking

### Spawning Tasks

<Listing number="17-6" caption="Spawning an async task" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-06/src/main.rs:all}}
```

</Listing>

Like `thread::spawn`, `trpl::spawn_task` runs code concurrently. Unlike threads:
- Tasks run on the same thread (unless using a multi-threaded runtime)
- Much lower overhead - millions of tasks are feasible
- Terminated when the runtime shuts down

### Joining Tasks

Tasks return join handles that implement `Future`:

<Listing number="17-7" caption="Awaiting task completion" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-07/src/main.rs:handle}}
```

</Listing>

### Joining Multiple Futures

`trpl::join` combines futures and waits for all to complete:

<Listing number="17-8" caption="Using join to await multiple futures" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-08/src/main.rs:join}}
```

</Listing>

Key points about `join`:
- **Fair scheduling**: alternates between futures equally
- **Deterministic ordering**: consistent execution pattern
- **Structured concurrency**: all futures complete before continuing

Compare this to threads where the OS scheduler determines execution order non-deterministically.

### Message Passing

Async channels work similarly to sync channels but with async methods:

<Listing number="17-9" caption="Basic async channel usage" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-09/src/main.rs:channel}}
```

</Listing>

The `recv` method returns a `Future` that resolves when a message arrives.

#### Multiple Messages with Delays

<Listing number="17-10" caption="Sending multiple messages with delays" file-name="src/main.rs">

```rust,ignore
{{#rustdoc_include ../listings/ch17-async-await/listing-17-10/src/main.rs:many-messages}}
```

</Listing>

This runs serially - all sends complete before receives start. For concurrency, separate into different futures:

<Listing number="17-11" caption="Concurrent sending and receiving" file-name="src/main.rs">

```rust,ignore
{{#rustdoc_include ../listings/ch17-async-await/listing-17-11/src/main.rs:futures}}
```

</Listing>

### Channel Cleanup

Channels need proper cleanup to avoid infinite loops. Move the sender into an async block so it gets dropped when sending completes:

<Listing number="17-12" caption="Using async move for proper cleanup" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-12/src/main.rs:with-move}}
```

</Listing>

The `move` keyword transfers ownership into the async block. When the block completes, `tx` is dropped, closing the channel.

### Multiple Producers

Clone the sender for multiple producers:

<Listing number="17-13" caption="Multiple async message producers" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-13/src/main.rs:here}}
```

</Listing>

Use `trpl::join3` for three futures, or the `join!` macro for variable numbers.

## Patterns Summary

**Task Spawning**: `spawn_task` for fire-and-forget concurrency
**Joining**: `join`/`join!` for structured concurrency waiting on all futures
**Channels**: Async message passing with `trpl::channel`
**Cleanup**: Use `async move` to ensure proper resource cleanup

[thread-spawn]: ch16-01-threads.html#creating-a-new-thread-with-spawn
[join-handles]: ch16-01-threads.html#waiting-for-all-threads-to-finish-using-join-handles
[message-passing-threads]: ch16-02-message-passing.html
[if-let]: ch06-03-if-let.html
[capture-or-move]: ch13-01-closures.html#capturing-references-or-moving-ownership
[move-threads]: ch16-01-threads.html#using-move-closures-with-threads
