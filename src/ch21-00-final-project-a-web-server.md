# Final Project: Building a Multithreaded Web Server

This capstone project demonstrates systems programming concepts by building an HTTP server from scratch, integrating threading, I/O, and error handling.

## Project Goals

**Core Implementation**:
1. TCP socket programming and HTTP protocol handling
2. Request parsing and response generation  
3. Multithreaded architecture with thread pool
4. Graceful shutdown and resource management

**Learning Objectives**:
- Low-level network programming
- Concurrent request handling
- Thread pool design patterns
- Systems programming best practices

## Why Build From Scratch

**Educational Value**: Understanding fundamentals before using production libraries
**Systems Programming**: Working at the TCP/HTTP level rather than framework abstractions
**Performance Insights**: Learning how async/threaded servers work internally

## Architecture Overview

```rust
// High-level structure
struct ThreadPool { /* worker threads */ }
struct HttpServer { /* socket listener */ }

// Request flow: TCP -> HTTP parsing -> Response -> Send
```

## Production vs Learning

**Production**: Use established crates (tokio, hyper, actix-web)
**This Project**: Educational implementation for understanding concepts

**Why not async here**: Thread pools are complex enough without adding async runtime complexity. Chapter 17 covered async patterns.

This project consolidates knowledge from previous chapters into a practical systems programming application, demonstrating Rust's capabilities for building efficient, safe concurrent systems.

For our final project, we'll make a web server that says "hello" and looks like
Figure 21-1 in a web browser.

![hello from rust](img/trpl21-01.png)

<span class="caption">Figure 21-1: Our final shared project</span>

Here is our plan for building the web server:

1. Learn a bit about TCP and HTTP.
2. Listen for TCP connections on a socket.
3. Parse a small number of HTTP requests.
4. Create a proper HTTP response.
5. Improve the throughput of our server with a thread pool.

Before we get started, we should mention two details. First, the method we'll
use won't be the best way to build a web server with Rust. Community members
have published a number of production-ready crates available on
[crates.io](https://crates.io/) that provide more complete web server and thread
pool implementations than we'll build. However, our intention in this chapter is
to help you learn, not to take the easy route. Because Rust is a systems
programming language, we can choose the level of abstraction we want to work
with and can go to a lower level than is possible or practical in other
languages.

Second, we will not be using async and await here. Building a thread pool is a
big enough challenge on its own, without adding in building an async runtime!
However, we will note how async and await might be applicable to some of the
same problems we will see in this chapter. Ultimately, as we noted back in
Chapter 17, many async runtimes use thread pools for managing their work.

We'll therefore write the basic HTTP server and thread pool manually so you can
learn the general ideas and techniques behind the crates you might use in the
future.
