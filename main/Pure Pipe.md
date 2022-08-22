Tags: #frontend #angular 
Created: 2022-08-22 20:08
References: [[Transforming Data Using Pipes]]

# Pure Pipe
By default, [[Angular]] executes the pipe function only **when it detects that any input parameter has changed**. This type of pipes are called pure [[Pipe]]s. To make a pipe impure, you can set the `pure` property of the `@Pipe` decorator to `false`:
```ts
@Pipe({
  name: 'flyingHeroesImpure',
  pure: false
})
```