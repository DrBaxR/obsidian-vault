Tags: #rust
Created: 2022-10-01 14:10
References: https://doc.rust-lang.org/book/ch13-01-closures.html https://youtu.be/dHkzSZnYXmk

# Rust Closures
[[Rust]] closures are anonymous functions that enclose their environment when they were created.

Thet generally don't require you to annotate the types used within them, since the compiler can easily infer them. You are however expected toannotate the parameter types and return type when this inference is not possible.

```rust
let expensive_closure = |num: u32| -> u32 {
	println!("calculating slowly...");
	thread::sleep(Duration::from_secs(2));
	num
};
```

## Capturing environment
There are three ways closures can capture their environment: by *borrowing immutably*, *borrowing mutable* or *moving* the values.

The first two ways can be inferred by the compiler, however if you want to move the values used in the closure, you need to use the `move` keyword when declaring the closure:

```rust
use std::thread;

fn main() {
    let list = vec![1, 2, 3];
    println!("Before defining closure: {:?}", list);

    thread::spawn(move || println!("From thread: {:?}", list))
        .join()
        .unwrap();
}
```

## Function traits
Closures automatically implement one, two or all three of the following traits basedon the way they handle values from the environment:
- `FnOnce` - functions that can be called once because they move the captured values in the body
- `FnMut` - closures that don't move captured values in their body but that might mutate the captured values. These can be called more than once, but not more than once at a time
- `Fn` - closures that only immutably borrow the captured values. These don't have any restrictions