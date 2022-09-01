Tags: #backend 
Created: 2022-09-01 15:09
References: [[Jersey Filters and Interceptors]]

# Jersey Filters
[[Jersey]] filters let you modify the properties of requests and responses. They are always executed, regardless of whether the resource was found or not.

They split in two categories: [[Request Filter]]s and [[Response Filter]]s.

For each of those, you need to implement an interface and annotate it with `@Provider`.