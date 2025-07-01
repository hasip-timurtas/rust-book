## Building a Single-Threaded Web Server

### TCP Connection Handling

```rust,editable,no_run
use std::net::TcpListener;

let listener = TcpListener::bind("127.0.0.1:7878")?;
for stream in listener.incoming() {
    let stream = stream?;
    handle_connection(stream);
}
```

Similar to Node.js `server.listen()`, but blocking by default.

### HTTP Request Structure

```
GET / HTTP/1.1
Host: 127.0.0.1:7878
User-Agent: Mozilla/5.0...

[optional body]
```

- Method + URI + Version
- Headers until empty line
- Optional body

### Basic Response

```rust,editable,no_run
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];
    stream.read(&mut buffer)?;
    
    let response = "HTTP/1.1 200 OK\r\n\r\nHello, World!";
    stream.write(response.as_bytes())?;
    stream.flush()?;
}
```

### Serving Files

```rust,editable,no_run
fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];
    stream.read(&mut buffer)?;
    
    let get = b"GET / HTTP/1.1\r\n";
    let (status_line, filename) = if buffer.starts_with(get) {
        ("HTTP/1.1 200 OK", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND", "404.html")
    };
    
    let contents = fs::read_to_string(filename)?;
    let response = format!(
        "{}\r\nContent-Length: {}\r\n\r\n{}",
        status_line,
        contents.len(),
        contents
    );
    
    stream.write(response.as_bytes())?;
    stream.flush()?;
}
```

### Performance Limitation

```rust,editable,no_run
// Simulate slow endpoint
if buffer.starts_with(b"GET /sleep HTTP/1.1\r\n") {
    thread::sleep(Duration::from_secs(5));
}
```

**Problem**: Single-threaded blocking - all requests wait for slow operations.

**Node.js difference**: Event loop handles I/O asynchronously on single thread, while this blocks the entire server.

**Next**: Thread pool for concurrent request handling.
