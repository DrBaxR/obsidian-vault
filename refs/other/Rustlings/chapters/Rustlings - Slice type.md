Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Slice type
A slice lets you reference a contiguous sequence of elements in a collection rather than the whole collection.

### String slices
You can get a section of the string using string slices (which have a type of `&str`).

```rust
fn main() {
	let s = String::from("hello");
	
	let len = s.len();
	
	let slice = &s[3..len];
	let slice = &s[3..];
}
```

## Resources
[[Rustlings]]