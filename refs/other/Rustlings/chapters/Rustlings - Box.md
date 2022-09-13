Tags: #reference 
Created: 2022-09-11 18:09

# Rustlings - Box
*Box* is a smart pointer that allows you to store data on the heap rather than the stack. The only thing that remains on the stack is the pointer to the heap data.

```rust
fn main() {
    let b = Box::new(5);
    println!("b = {}", b);
}
```

A common use case for boxes are recursive types.

The only thing that boxes provide are the indirection adn the heap allocation. When a `Box<T>` values goes out of scope, the heap data that the box is pointing to is cleaned up as well.

## Resources
[[Rustlings]]