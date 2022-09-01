Tags: #backend 
Created: 2022-09-01 15:09
References: [[Jersey Filters and Interceptors]]

# Name Binding
Name binding makes it possible for [[Jersey Filters]] and [[Jersey Interceptors]] to onle be executed for certain resources.

There are two ways to acheive name binding: static (where you use annotations) and dynamic (where you manually - or programatically - provide them for each resource in your application).