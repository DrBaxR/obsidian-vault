**Tags: #reference 
Created: 2022-09-11 18:09

# Rustlings - Shared-state concurrency
### Mutex
A mutex is a mechanism that lets you share data among multiple threads by using the data as a lock: *Imagine a panel discussion at a conference with only one microphone. Before a panelist can speak, they have to ask or signal that they want to use the microphone*.

```rust
use std::sync::Mutex;

fn main() {
    let m = Mutex::new(5);

    {
        let mut num = m.lock().unwrap();
        *num = 6;
    }

    println!("m = {:?}", m);
}
```

`lock` acquires the data from the mutex and it fails in case another thread holding the lock panicked.

The thing that `lock` returns is actually a smart pointer, which, once it goes out of scope, makes the lock get released.

### Atomic reference couting
`Arc<T>` is a type like `Rc<T>` ([[Reference Counted]]) that is safe to use in concurrent situations, the *a* stands for *atomic*.

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

## Resources
[[Rustlings]]