## Streams: Async Iterators

Streams provide async iteration over sequences of values. They're the async equivalent of `Iterator`, yielding items over time rather than immediately.

### Core Concepts

**Stream**: Async equivalent of `Iterator`
**StreamExt**: Extension trait providing utility methods (like `IteratorExt`)

Basic stream from iterator:

<Listing number="17-30" caption="Creating a stream from an iterator (doesn't compile)" file-name="src/main.rs">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-async-await/listing-17-30/src/main.rs:stream}}
```

</Listing>

**Error**: Missing `StreamExt` trait for the `next()` method.

<Listing number="17-31" caption="Importing StreamExt for stream methods" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-31/src/main.rs:all}}
```

</Listing>

### Stream Processing

Streams support familiar iterator patterns:

<Listing number="17-32" caption="Filtering streams with StreamExt methods" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-32/src/main.rs:all}}
```

</Listing>

### Real-time Data Streams

Streams excel at processing real-time data like WebSocket messages or event streams:

<Listing number="17-33" caption="Message stream with ReceiverStream" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-33/src/main.rs:all}}
```

</Listing>

`ReceiverStream` converts async channel receivers into streams.

### Timeouts on Streams

Apply timeouts to individual stream items:

<Listing number="17-34" caption="Stream timeouts with StreamExt::timeout" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-34/src/main.rs:timeout}}
```

</Listing>

Add delays to test timeout behavior:

<Listing number="17-35" caption="Variable delays in message stream" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-35/src/main.rs:messages}}
```

</Listing>

**Key point**: Use `spawn_task` to avoid blocking the stream creation. The async delays happen concurrently with stream consumption.

### Interval Streams

Create periodic streams for regular events:

<Listing number="17-36" caption="Creating a periodic interval stream" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-36/src/main.rs:intervals}}
```

</Listing>

### Merging Streams

Combine multiple streams into one:

<Listing number="17-37" caption="Attempting to merge streams (doesn't compile)" file-name="src/main.rs">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-async-await/listing-17-37/src/main.rs:main}}
```

</Listing>

**Error**: Type mismatch between streams. Need to align types:

<Listing number="17-38" caption="Type alignment for stream merging" file-name="src/main.rs">

```rust,ignore
{{#rustdoc_include ../listings/ch17-async-await/listing-17-38/src/main.rs:main}}
```

</Listing>

### Stream Rate Control

Control stream throughput with `throttle` and limit with `take`:

<Listing number="17-39" caption="Throttling and limiting merged streams" file-name="src/main.rs">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-39/src/main.rs:throttle}}
```

</Listing>

**Throttle**: Limits polling rate, not just output filtering
**Take**: Limits total number of items processed
**Lazy evaluation**: Unprocessed items are never generated (performance benefit)

### Error Handling

Handle stream errors gracefully:

<Listing number="17-40" caption="Proper error handling in streams">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-40/src/main.rs:errors}}
```

</Listing>

## Stream Patterns

**Data Processing**: Transform async data with `map`, `filter`, `fold`
**Rate Limiting**: Use `throttle` to control processing rate
**Merging**: Combine multiple event sources with `merge`
**Timeouts**: Apply deadlines to individual stream items
**Backpressure**: Control memory usage with `take` and buffering

Key differences from synchronous iterators:
- Items arrive over time (not immediately available)
- Support timeouts and cancellation
- Enable concurrent processing of multiple streams
- Natural fit for event-driven architectures

**Performance consideration**: Streams are lazy - items are only produced when polled. This enables efficient resource usage and backpressure handling.

[17-02-messages]: ch17-02-concurrency-with-async.html#message-passing
[iterator-trait]: ch13-02-iterators.html#the-iterator-trait-and-the-next-method
