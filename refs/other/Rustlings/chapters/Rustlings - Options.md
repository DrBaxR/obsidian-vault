Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Options
The `Option` enum is the way rust deals will nullable values and many more:
-   Initial values
-   Return values for functions that are not defined over their entire input range (partial functions)
-   Return value for otherwise reporting simple errors, where None is returned on error
-   Optional struct fields
-   Struct fields that can be loaned or "taken"
-   Optional function arguments
-   Nullable pointers
-   Swapping things out of difficult situations

It is really common to use things like `if let` and `while let` with optionals.

## Resources
[[Rustlings]]