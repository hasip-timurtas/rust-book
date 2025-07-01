## Using Threads to Run Code Simultaneously

Rust uses a 1:1 threading model where each language thread maps to an OS thread. Thread creation uses `thread::spawn` with closures containing the code to execute.

### Creating Threads with `spawn`

<Listing number="16-1" file-name="src/main.rs" caption="Creating a new thread to print one thing while the main thread prints something else">

```rust,editable
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-01/src/main.rs}}
```

</Listing>

Key behaviors:
- Main thread completion terminates all spawned threads
- Thread execution order is non-deterministic
- `thread::sleep` yields execution to other threads

### Waiting for Thread Completion with `join`

`thread::spawn` returns a `JoinHandle<T>` that provides a `join` method to wait for thread completion:

<Listing number="16-2" file-name="src/main.rs" caption="Saving a `JoinHandle<T>` from `thread::spawn` to guarantee the thread is run to completion">

```rust,editable
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-02/src/main.rs}}
```

</Listing>

Calling `join` blocks the current thread until the target thread terminates. Placement of `join` calls affects concurrency behavior—early joins eliminate parallelism.

### Using `move` Closures for Data Transfer

To transfer ownership of variables from one thread to another, use `move` closures:

<Listing number="16-3" file-name="src/main.rs" caption="Attempting to use a vector created by the main thread in another thread">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-03/src/main.rs}}
```

</Listing>

This fails because Rust cannot guarantee the lifetime of borrowed data across thread boundaries:

```console
{{#include ../listings/ch16-fearless-concurrency/listing-16-03/output.txt}}
```

The `move` keyword forces the closure to take ownership:

<Listing number="16-5" file-name="src/main.rs" caption="Using the `move` keyword to force a closure to take ownership of the values it uses">

```rust,editable
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-05/src/main.rs}}
```

</Listing>

This transfers ownership to the spawned thread, preventing use-after-free errors and ensuring thread safety through compile-time checks.

[capture]: ch13-01-closures.html#capturing-the-environment-with-closures
