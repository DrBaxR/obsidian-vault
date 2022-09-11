Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Generics
There are multiple places where you can use generics. Here they are:
- Function definitions
- Struct definitions
- Enum definitions
- Method definitions

```rust
struct Wrapper<T> {
    value: T,
}
  
impl<T> Wrapper<T> {
    pub fn new(value: T) -> Self {
        Wrapper { value }
    }
}
```

### Bounds
It is possible to specify what functionality a generic implements by using **bounds**.

```rust
// Define a function `printer` that takes a generic type `T` which
// must implement trait `Display`.
fn printer<T: Display>(t: T) {
    println!("{}", t);
}
```

## Resources
[[Rustlings]]