## Futures and the Async Syntax

Rust's async programming centers on **futures** - values that represent work that may complete later. The `Future` trait provides a common interface for async operations, similar to Promises in JavaScript.

Key concepts:
- `async` functions/blocks return futures
- `await` polls futures to completion
- Futures are lazy - they do nothing until awaited
- Each await point is where execution can pause/resume

```rust,ignore
async fn fetch_title(url: &str) -> Option<String> {
    let response = http_get(url).await;  // Yield control here
    let html = response.text().await;    // And here
    parse_title(&html)
}
```

### Setup

Create a new project and add the `trpl` crate, which provides runtime and utility functions:

```console
$ cargo new hello-async && cd hello-async
$ cargo add trpl
```

## First Async Program: Web Scraper

<Listing number="17-1" file-name="src/main.rs" caption="Async function to extract page title">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-01/src/main.rs:all}}
```

</Listing>

Key points:
- `async fn` returns `impl Future<Output = ReturnType>`
- `await` is postfix: `future.await` not `await future`
- Network and parsing operations yield control at await points
- Chaining with `await` creates readable async code:

<Listing number="17-2" file-name="src/main.rs" caption="Method chaining with await">

```rust
{{#rustdoc_include ../listings/ch17-async-await/listing-17-02/src/main.rs:chaining}}
```

</Listing>

### Async Function Compilation

The compiler transforms:
```rust
async fn page_title(url: &str) -> Option<String> { /* body */ }
```

Into roughly:
```rust
fn page_title(url: &str) -> impl Future<Output = Option<String>> {
    async move { /* body */ }
}
```

## Running Async Code

`main` cannot be `async` - you need a runtime to execute futures:

<Listing number="17-3" caption="Attempted async main (doesn't compile)" file-name="src/main.rs">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-async-await/listing-17-03/src/main.rs:main}}
```

</Listing>

Fix with `trpl::run`:

<Listing number="17-4" caption="Using trpl::run to execute async code" file-name="src/main.rs">

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch17-async-await/listing-17-04/src/main.rs:run}}
```

</Listing>

The runtime manages the state machine that Rust creates from async blocks. Each await point represents where the state machine can pause and resume.

### Racing Multiple Futures

<Listing number="17-5" caption="Racing two HTTP requests" file-name="src/main.rs">

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch17-async-await/listing-17-05/src/main.rs:all}}
```

</Listing>

`trpl::race` returns `Either<Left(A), Right(B)>` indicating which future completed first. Unlike threads, the order of argument evaluation affects which future starts first (non-fair scheduling).

## Runtime and State Machines

Async blocks compile to state machines. The runtime (executor) manages these machines:

```rust
enum PageTitleState {
    Start,
    WaitingForResponse,
    WaitingForText,
    Complete,
}
```

At each await point, the future yields control back to the runtime. The runtime can then:
- Schedule other futures
- Handle IO completion
- Resume futures when their dependencies are ready

This cooperative multitasking requires explicit yield points (await) - CPU-intensive work without await points will block other futures.

[impl-trait]: ch10-02-traits.html#traits-as-parameters
[iterators-lazy]: ch13-02-iterators.html
[thread-spawn]: ch16-01-threads.html#creating-a-new-thread-with-spawn
[cli-args]: ch12-01-accepting-command-line-arguments.html
[crate-source]: https://github.com/rust-lang/book/tree/main/packages/trpl
[futures-crate]: https://crates.io/crates/futures
[tokio]: https://tokio.rs
