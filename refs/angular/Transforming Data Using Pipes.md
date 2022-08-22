Tags: #reference 
Created: 2022-08-22 20:08

# Transforming Data Using Pipes
[[Angular]] [[Pipe]]s are simple functions that you can use in template variables to accept an input value and return a transformed value.

In order to use a pipe in the template, you can use the pipe `|` character:
```html
<p>The hero's birthday is {{ birthday | date }}</p>
```

It is also possible to chains these pipes, each one processing the transformed value of the previous one in the chain. It is also possible to have pipes receive multiple inputs. To do this, you need to separate each of the inputs by `:` when you use the pipe:
```html
{{ amount | [currency](https://angular.io/api/common/CurrencyPipe):'EUR':'Euros '}}
```

By default, [[Angular]] executes the pipe function only **when it detects that any input parameter has changed**. This type of pipes are called [[Pure Pipe]]s. To make a pipe impure, you can set the `pure` property of the `@Pipe` decorator to `false`:
```ts
@Pipe({
  name: 'flyingHeroesImpure',
  pure: false
})
```

You can use the built-in *impure* pipe called `async` to subscribe to observables (or to *unwrap* their values).

Pipes can also be used to cache [[HTTP]] requests. This is an example, not a built-in pipe that [[Angular]] offers. You can create a pipe that only makes a call to the [[API]] when the URL changes:
```ts
import { HttpClient } from '@angular/common/http';
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'fetch',
  pure: false
})
export class FetchJsonPipe implements PipeTransform {
  private cachedData: any = null;
  private cachedUrl = '';

  constructor(private http: HttpClient) { }

  transform(url: string): any {
    if (url !== this.cachedUrl) {
      this.cachedData = null;
      this.cachedUrl = url;
      this.http.get(url).subscribe(result => this.cachedData = result);
    }

    return this.cachedData;
  }
}
```

## Resources
https://angular.io/guide/pipes