Tags: #frontend #angular 
Created: 2022-08-22 20:08
References: [[Transforming Data Using Pipes]]

# Pipe
[[Angular]] pipes are simple functions that you can use in template variables to accept an input value and return a transformed value.

In order to use a pipe in the template, you can use the pipe `|` character:
```html
<p>The hero's birthday is {{ birthday | date }}</p>
```

It is also possible to chains these pipes, each one processing the transformed value of the previous one in the chain. It is also possible to have pipes receive multiple inputs. To do this, you need to separate each of the inputs by `:` when you use the pipe:
```html
{{ amount | [currency](https://angular.io/api/common/CurrencyPipe):'EUR':'Euros '}}
```