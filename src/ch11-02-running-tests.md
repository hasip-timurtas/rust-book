## Controlling How Tests Are Run

`cargo test` compiles and runs tests with customizable behavior through command-line options. Options for `cargo test` come before `--`, while options for the test binary come after `--`.

### Parallel vs Sequential Execution

Tests run in parallel by default using threads. To run sequentially or control thread count:

```console
$ cargo test -- --test-threads=1
```

### Showing Function Output

By default, passing tests capture output (e.g., `println!`). Failed tests show captured output. To see output from passing tests:

```console
$ cargo test -- --show-output
```

Example test with output:

<Listing number="11-10" file-name="src/lib.rs" caption="Tests with `println!` output">

```rust,editable,panics,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-10/src/lib.rs}}
```

</Listing>

### Running Subset of Tests

#### Single Tests

Run specific test by name:

```console
$ cargo test one_hundred
```

#### Filtering Tests

Run multiple tests matching a pattern:

```console
$ cargo test add  // Runs tests with "add" in the name
```

Module names are part of test names, so you can filter by module.

### Ignoring Tests

Mark expensive tests to skip during normal runs:

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-11-ignore-a-test/src/lib.rs:here}}
```

Run only ignored tests:

```console
$ cargo test -- --ignored
```

Run all tests (including ignored):

```console
$ cargo test -- --include-ignored
```
