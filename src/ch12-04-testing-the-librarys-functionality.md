## Test-Driven Development

With logic separated into lib.rs, we can implement the search functionality using TDD.

### Writing the Test

<Listing number="12-15" file-name="src/lib.rs" caption="Creating test for the search function">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-15/src/lib.rs:here}}
```

</Listing>

### Implementing to Pass

<Listing number="12-16" file-name="src/lib.rs" caption="Minimal implementation to avoid panic">

```rust,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-16/src/lib.rs:here}}
```

</Listing>

#### Lifetime Annotations

The function signature requires explicit lifetime annotation because the returned string slices reference the `contents` parameter, not `query`. This tells Rust that the returned data lives as long as the `contents` input.

### Implementation Steps

#### Iterating Through Lines

<Listing number="12-17" file-name="src/lib.rs" caption="Line-by-line iteration">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-17/src/lib.rs:here}}
```

</Listing>

#### Filtering Lines

<Listing number="12-18" file-name="src/lib.rs" caption="Adding contains check">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-18/src/lib.rs:here}}
```

</Listing>

#### Collecting Results

<Listing number="12-19" file-name="src/lib.rs" caption="Complete implementation">

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-19/src/lib.rs:here}}
```

</Listing>

The implementation works with basic text search. Chapter 13 will show how to improve this using iterators for better performance and cleaner code.

[validating-references-with-lifetimes]: ch10-03-lifetime-syntax.html#validating-references-with-lifetimes
[ch11-anatomy]: ch11-01-writing-tests.html#the-anatomy-of-a-test-function
[ch10-lifetimes]: ch10-03-lifetime-syntax.html
[ch3-iter]: ch03-05-control-flow.html#looping-through-a-collection-with-for
[ch13-iterators]: ch13-02-iterators.html
