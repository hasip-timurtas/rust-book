## Comparing Performance: Loops vs. Iterators

Benchmark results comparing explicit `for` loops vs. iterator implementation searching for "the" in *The Adventures of Sherlock Holmes*:

```text
test bench_search_for  ... bench:  19,620,300 ns/iter (+/- 915,700)
test bench_search_iter ... bench:  19,234,900 ns/iter (+/- 657,200)
```

**Key findings:**
- Iterator version performs slightly better
- Performance difference is negligible  
- Both compile to nearly identical assembly

## Zero-Cost Abstractions

Iterators exemplify Rust's zero-cost abstractions—high-level constructs that impose no runtime overhead. The compiler applies optimizations including:

- Loop unrolling
- Bounds check elimination  
- Inline expansion
- Dead code elimination

These optimizations often produce assembly equivalent to hand-optimized code.

## Summary

Closures and iterators provide functional programming patterns with systems-level performance. They enable expressive, maintainable code without sacrificing runtime efficiency—a core principle of Rust's design philosophy.

Use iterators and closures confidently in performance-critical code. The abstractions compile away, leaving only the essential operations.

Now that we've improved the expressiveness of our I/O project, let's look at
some more features of `cargo` that will help us share the project with the
world.
