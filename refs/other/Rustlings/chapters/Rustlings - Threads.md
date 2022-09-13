Tags: #reference 
Created: 2022-09-13 19:09

# Rustlings - Threads
## Basics
To create a thread, you can call `thread::spawn` and pass in a [[Closure]], which is the thing that the thread is going to do. This `spawn` function is going to return a handler, on which you can call `join` to wait for that thread to finish executing.

In order to make the thread get ownership of the data the closure uses, you need to use the `move` keyword before the closure:

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("Here's a vector: {:?}", v);
    });

    handle.join().unwrap();
}
```

## Message passing
*Do not communicate by sharing memory; instead, share memory by communicating*.

You can create a channel using `mpsc::channel()`, which will give you a tuple with a receiver and a transmitter. You can use the transmitter in a thread to send data to the main thread.

```rust
let (tx, rx) = mpsc::channel();

thread::spawn(move || {
let messages = vec![
  String::from("first"),
  String::from("second"),
  String::from("third"),
  String::from("fourth"),
  String::from("fifth")
];

for msg in messages {
  tx.send(msg).unwrap();
  thread::sleep(Duration::from_secs(1));
}
});

for msg in rx {
	println!("Message: {}", msg);
}
```

To receive a message in the main thread, you can use `rx.recv()`, which is blocking the thread until a message arrives in the channel, or `rx.try_recv()`, which is non-blocking.

Since `mpsc` stands for *multi producer, single consumer*, it is possible to have multiple threads send messages in the transmitter. In order to receive another transmitter that you can move in another thread, you can call `tx.clone()` on the original transmitter.

## Shared state concurrency
*Mutex*es are a way to share data batween a thread. You can call `lock` on the mutex in order to wait to receive access to the value inside it.

In order to share mutexes between multiple threads, you can use `Arc`, which is a smart pointer that does what `Rc` does, only difference being that is is thread safe.

```rust
use std::sync::{mpsc, Mutex, Arc};
use std::thread;
  
fn main() {
  let counter = Arc::new(Mutex::new(0));
  let mut handlers = vec![];
  
  for _ in 0..10 {
    let counter = Arc::clone(&counter);
    let handle = thread::spawn(move || {
      let mut val = counter.lock().unwrap();
      *val += 1;
    });
  
    handlers.push(handle)
  }
  
  for handler in handlers {
    handler.join().unwrap();
  }
  
  println!("Final value: {}", counter.lock().unwrap())
}
```

## Resources
[[Rustlings]]