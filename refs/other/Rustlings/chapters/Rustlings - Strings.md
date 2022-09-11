Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Strings
You can create a string with `String::new` or `String::from`. Concatenation can be done with `+` or with the `format!` macro.

A `String` is a wrapper over `Vec<u8>`. There are some problems in case your string does not have 8 byte encoed characters, more info in documentation if you ever need this.

There is also another string type `&str`, which is a slice of a string.

## Resources
[[Rustlings]]