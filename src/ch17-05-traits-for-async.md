## Async Traits Deep Dive

Understanding the underlying traits enables effective async programming and troubleshooting.

### The Future Trait

```rust
use std::pin::Pin;
use std::task::{Context, Poll};

pub trait Future {
    type Output;

    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
```

**Key components**:
- `Output`: The value produced when the future completes
- `poll()`: State machine driver, called by the runtime
- `Poll<T>`: Either `Ready(T)` or `Pending`

```rust
enum Poll<T> {
    Ready(T),
    Pending,
}
```

**Runtime interaction**: `await` compiles to repeated `poll()` calls in a loop:

```rust,ignore
let mut page_title_fut = page_title(url);
loop {
    match page_title_fut.poll(cx) {
        Ready(value) => return value,
        Pending => {
            // Runtime schedules other work, resumes later
        }
    }
}
```

The `Context` parameter enables runtime coordination - futures register wake-up conditions.

### Pin and Unpin

**Problem**: Async state machines can contain self-references. Moving such structures breaks internal pointers.

```rust
// Simplified state machine representation
struct AsyncStateMachine {
    state: State,
    data: String,
    reference_to_data: Option<&String>, // Self-reference!
}
```

**Solution**: `Pin<T>` prevents moving the pointed-to value.

```rust
// Pin prevents the inner value from moving
let pinned: Pin<Box<AsyncStateMachine>> = Box::pin(state_machine);
```

**`Unpin` marker trait**: Types safe to move even when pinned. Most types implement `Unpin` automatically.

```rust
// Most types can be moved freely
let string = String::from("hello");
let pinned_string = Pin::new(&mut string); // OK - String: Unpin

// Self-referential futures need pinning
let future = async { /* complex state machine */ };
let pinned_future = Box::pin(future); // Required for join_all()
```

**Practical implications**:
- Most code doesn't deal with `Pin` directly
- `join_all()` requires `Unpin` because it stores futures in collections
- Use `Box::pin()` or `pin!()` macro when required
- Stack allocation (`pin!`) vs heap allocation (`Box::pin`)

```rust
use std::pin::pin;

// Stack pinning (preferred when lifetime allows)
let fut = pin!(async_operation());

// Heap pinning (needed for collections or longer lifetimes)
let fut = Box::pin(async_operation());
```

### The Stream Trait

Streams combine `Iterator` and `Future` concepts:

```rust
use std::pin::Pin;
use std::task::{Context, Poll};

trait Stream {
    type Item;

    fn poll_next(
        self: Pin<&mut Self>,
        cx: &mut Context<'_>
    ) -> Poll<Option<Self::Item>>;
}
```

**Comparison**:
- `Iterator::next()` → `Option<Item>` (synchronous)
- `Future::poll()` → `Poll<Output>` (async ready/pending)
- `Stream::poll_next()` → `Poll<Option<Item>>` (async sequence)

**StreamExt trait**: Provides high-level methods like `next()`, `map()`, `filter()`:

```rust
// Simplified StreamExt implementation
trait StreamExt: Stream {
    async fn next(&mut self) -> Option<Self::Item> 
    where 
        Self: Unpin
    {
        // Implementation uses poll_next() internally
    }

    fn map<F, U>(self, f: F) -> Map<Self, F> 
    where 
        F: FnMut(Self::Item) -> U
    {
        // Returns a new stream that applies f to each item
    }
}
```

## Practical Guidelines

**Future trait**: Rarely implemented directly. Use `async fn` and `async {}` blocks.

**Pin/Unpin**: 
- Most types are `Unpin` (can be moved freely)
- Async blocks often require pinning for collections
- Use `pin!()` for stack allocation, `Box::pin()` for heap

**Stream trait**:
- Use `StreamExt` methods for stream operations
- Similar to iterator patterns but async
- Enables backpressure and cancellation

**Error patterns**:
```rust
// Error: Missing StreamExt
let mut stream = some_stream();
let item = stream.next().await; // ❌ No method `next`

// Fix: Import StreamExt
use futures::StreamExt;
let item = stream.next().await; // ✅ Works

// Error: Unpin requirement
let futures = vec![Box::new(async_block())]; // ❌ Unpin not satisfied

// Fix: Pin the futures
let futures = vec![Box::pin(async_block())]; // ✅ Works
```

**When to use each**:
- **Future**: For single async operations
- **Stream**: For async sequences over time  
- **Pin**: When collections or trait objects are involved
- **StreamExt**: For stream transformations and operations

The traits form a coherent system where:
1. `Future` provides the foundation for async operations
2. `Pin` enables safe self-referential state machines
3. `Stream` extends futures to sequences
4. Extension traits provide ergonomic APIs

[ch-18]: ch18-00-oop.html
[async-book]: https://rust-lang.github.io/async-book/
[under-the-hood]: https://rust-lang.github.io/async-book/02_execution/01_chapter.html
[pinning]: https://rust-lang.github.io/async-book/04_pinning/01_chapter.html
[first-async]: ch17-01-futures-and-syntax.html#our-first-async-program
[any-number-futures]: ch17-03-more-futures.html#working-with-any-number-of-futures
