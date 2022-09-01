Tags: #backend 
Created: 2022-09-01 15:09
References: [[Jersey Filters and Interceptors]]

# Jersey Interceptors
[[Jersey]] interceptors are a way to add stuff to the body of requests/responses. In order to use them you first need to implement an interface and provide the class implementing that interface. After doing that, you also need to register that class in the config of the client.

These interceptors are split in two categories: **reader interceptors** (for inbound streams) and **writer interceptors** (for outbound streams).