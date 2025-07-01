## Turning Our Single-Threaded Server into a Multithreaded Server

### Problem: Sequential Request Processing

The current server blocks on each request:

```rust,no_run
// /sleep endpoint demonstrates the blocking issue
if buffer.starts_with(b"GET /sleep HTTP/1.1\r\n") {
    thread::sleep(Duration::from_secs(5));
}
```

**Issue**: All requests wait behind slow operations. Unlike Node.js's event loop or async frameworks, this blocks the entire server.

### Solution: Thread Pool Pattern

**Thread pool benefits**:
- Fixed number of worker threads (prevents DoS via thread exhaustion)
- Work queue distributes tasks across available workers
- Graceful handling of concurrent requests

### Implementation

#### 1. Basic Threading (Naive Approach)

```rust,no_run
use std::thread;

for stream in listener.incoming() {
    let stream = stream.unwrap();
    thread::spawn(|| {
        handle_connection(stream);
    });
}
```

**Problem**: Unlimited thread creation can exhaust system resources.

#### 2. ThreadPool API Design

```rust,ignore
let pool = ThreadPool::new(4);

for stream in listener.incoming() {
    let stream = stream.unwrap();
    pool.execute(|| {
        handle_connection(stream);
    });
}
```

#### 3. ThreadPool Implementation

**Core structure**:
```rust
use std::sync::{mpsc, Arc, Mutex};

pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}

struct Worker {
    id: usize,
    thread: thread::JoinHandle<()>,
}

type Job = Box<dyn FnOnce() + Send + 'static>;
```

**Constructor with validation**:
```rust
impl ThreadPool {
    pub fn new(size: usize) -> ThreadPool {
        assert!(size > 0);
        
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));
        let mut workers = Vec::with_capacity(size);
        
        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }
        
        ThreadPool {
            workers,
            sender: Some(sender),
        }
    }
}
```

**Job execution**:
```rust
impl ThreadPool {
    pub fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.as_ref().unwrap().send(job).unwrap();
    }
}
```

**Worker implementation**:
```rust
impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv().unwrap();
            println!("Worker {id} got a job; executing.");
            job();
        });
        
        Worker { id, thread }
    }
}
```

### Key Architecture Patterns

**Channel-based work distribution**:
- `ThreadPool` holds sender
- Workers share receiver via `Arc<Mutex<Receiver>>`
- `Mutex` ensures single worker processes each job

**Type safety**:
- `Job` type alias for boxed closures
- `Send + 'static` bounds ensure thread safety
- `FnOnce` trait for single execution

**Resource management**:
- Pre-allocated worker threads
- Bounded concurrency (DoS protection)
- Work stealing via shared queue

### Performance Characteristics

**vs Single-threaded**: Concurrent request handling
**vs Unlimited threading**: Bounded resource usage
**vs Async I/O**: More memory per connection, but simpler mental model
**vs Node.js**: Multiple OS threads vs single-threaded event loop

**Note**: Modern async runtimes (tokio, async-std) often use similar thread pool patterns internally for CPU-bound work.
