## Graceful Shutdown and Cleanup

### Problem: Resource Cleanup

Current thread pool implementation doesn't clean up resources when dropping. Worker threads continue running indefinitely, preventing graceful shutdown.

### Solution: Implement `Drop` Trait

#### 1. Basic Drop Implementation

```rust
impl Drop for ThreadPool {
    fn drop(&mut self) {
        for worker in &mut self.workers {
            println!("Shutting down worker {}", worker.id);
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}
```

**Problem**: Workers are blocked on `recv()` calls - `join()` waits indefinitely.

#### 2. Signaling Shutdown

**Close channel to signal workers**:
```rust
impl Drop for ThreadPool {
    fn drop(&mut self) {
        // Drop sender to close channel
        drop(self.sender.take());
        
        // Wait for workers to finish
        for worker in &mut self.workers {
            println!("Shutting down worker {}", worker.id);
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}
```

**Update Worker to handle channel closure**:
```rust
impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let message = receiver.lock().unwrap().recv();
            
            match message {
                Ok(job) => {
                    println!("Worker {id} got a job; executing.");
                    job();
                }
                Err(_) => {
                    println!("Worker {id} disconnected; shutting down.");
                    break;
                }
            }
        });
        
        Worker { 
            id, 
            thread: Some(thread) 
        }
    }
}
```

#### 3. Updated Data Structures

**ThreadPool with Optional sender**:
```rust
pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}
```

**Worker with Optional thread**:
```rust
struct Worker {
    id: usize,
    thread: Option<thread::JoinHandle<()>>,
}
```

### Complete Implementation

**ThreadPool**:
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
    
    pub fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.as_ref().unwrap().send(job).unwrap();
    }
}

impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.take());
        
        for worker in &mut self.workers {
            println!("Shutting down worker {}", worker.id);
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}
```

### Testing Graceful Shutdown

**Limited server for testing**:
```rust
let pool = ThreadPool::new(4);

for stream in listener.incoming().take(2) {
    let stream = stream.unwrap();
    pool.execute(|| {
        handle_connection(stream);
    });
}

println!("Shutting down.");
```

**Expected output**:
```console
Worker 0 got a job; executing.
Worker 1 got a job; executing.
Shutting down.
Shutting down worker 0
Worker 0 disconnected; shutting down.
Worker 1 disconnected; shutting down.
Shutting down worker 1
...
```

### Key Patterns

**Resource cleanup sequence**:
1. Signal shutdown (close channel)
2. Wait for workers to finish current tasks
3. Join all worker threads

**Channel closure semantics**:
- Dropping sender closes channel
- `recv()` returns `Err` when channel closed
- Workers exit loop on channel closure

**RAII (Resource Acquisition Is Initialization)**:
- `Drop` trait ensures cleanup on scope exit
- Automatic resource management
- Exception safety through destructors

**Production considerations**:
- Timeout for join operations
- Graceful vs forceful shutdown
- Signal handling for shutdown triggers
