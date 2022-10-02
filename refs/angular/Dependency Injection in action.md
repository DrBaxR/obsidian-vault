Tags: #reference 
Created: 2022-10-02 15:10

# Dependency Injection in action
This explores many features of the [[Dependency Injection]] mechanism in [[Angular]].

## Sandboxing
Sandboxing is when each component has its own instance of a service. This is called sandboxing because each service and component has its own sandbox to play in.

This can be done by providing the injected service in the component's `@Component` decorator in the `providers` field.

## Make a dependency optional and limit search
This can be done using the `@Optional` and `@Host` decorators. The reason you might want to do this is in the case you project a component. In this case, the host of the projected component becomes the other component, therefore you can rely on the parent to provide certain services.

## Supply a custom provider
This can be done with `@Inject` and it allows you to use a custom provider to provide a concrete implementation for implicit dependencies.

```ts
import { Inject, Injectable, InjectionToken } from '@angular/core';

export const BROWSER_STORAGE = new InjectionToken<Storage>('Browser Storage', {
  providedIn: 'root',
  factory: () => localStorage
});

@Injectable({
  providedIn: 'root'
})
export class BrowserStorageService {
  constructor(@Inject(BROWSER_STORAGE) public storage: Storage) {}

  get(key: string) {
    return this.storage.getItem(key);
  }

  set(key: string, value: string) {
    this.storage.setItem(key, value);
  }

  remove(key: string) {
    this.storage.removeItem(key);
  }

  clear() {
    this.storage.clear();
  }
}
```

## Modify the provider search
This can be done with the `@Self` and `@SkipSelf` decorators and can be useful in cases when you want to provide the same dependency multiple times and define which injector handles the provider dependency.

As a continuation of the previous example:

```ts
import { Component, OnInit, Self, SkipSelf } from '@angular/core';
import { BROWSER_STORAGE, BrowserStorageService } from './storage.service';

@Component({
  selector: 'app-storage',
  template: `
    Open the inspector to see the local/session storage keys:

    <h3>Session Storage</h3>
    <button type="button" (click)="setSession()">Set Session Storage</button>

    <h3>Local Storage</h3>
    <button type="button" (click)="setLocal()">Set Local Storage</button>
  `,
  providers: [
    BrowserStorageService,
    { provide: BROWSER_STORAGE, useFactory: () => sessionStorage }
  ]
})
export class StorageComponent implements OnInit {

  constructor(
    @Self() private sessionStorageService: BrowserStorageService,
    @SkipSelf() private localStorageService: BrowserStorageService,
  ) { }

  ngOnInit() {
  }

  setSession() {
    this.sessionStorageService.set('hero', 'Dr Nice - Session');
  }

  setLocal() {
    this.localStorageService.set('hero', 'Dr Nice - Local');
  }
}
```

The first `BrowserStorageService` gets its `BROWSER_STORAGE` provider from the `StorageComponent`, because of the `@Self` decorator, while the second service uses the `BROWSER_STORAGE` provided at root level, since it is annotated with `@SkipSelf`, which skips the component's injector.

## Inject the component's DOM element
You can inject the element in a component/directive by using the `ElementRef` class in the constructor. This is how you would do it for a directive that highlights the element it is applied to when hovered:

```ts
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {

  @Input('appHighlight') highlightColor = '';

  private el: HTMLElement;

  constructor(el: ElementRef) {
    this.el = el.nativeElement;
  }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || 'cyan');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight('');
  }

  private highlight(color: string) {
    this.el.style.backgroundColor = color;
  }
}
```

An `ElementRef` is a wrapper around a [[DOM]] element. Usage:

```html
<div id="highlight"  class="di-component"  appHighlight>
  <h3>Hero Bios and Contacts</h3>
  <div appHighlight="yellow">
    <app-hero-bios-and-contacts></app-hero-bios-and-contacts>
  </div>
</div>
```

## Defining providers
There are multiple ways you can provide dependenciesL `useValue`, `useClass`, `useExisting` or `useFactory`.

### `useValue`
This lets you assogiate a fixed value with a DI token. Use to provide *runtime configuration constants* such as website base addresses and feature flags.

```ts
// NOTE: someHero is a value that is declared before and is immediately available
{ provide: Hero,          useValue:    someHero },
{ provide: TITLE,         useValue:   'Hero of the Month' },
```

First dependency is a class, while the second one is a special kind of lookup key, called an [[Injection Token]].

### `useClass`
You can use this type of provider to substitute an *alternative implementation* for a common or default class.

```ts
{ provide: HeroService,   useClass:    HeroService }, // 1
{ provide: LoggerService, useClass:    DateLoggerService }, // 2
```

First case is the *de-sugared* expanded form of the most common way you provide services. The second case substitutes `DateLoggerService` (which extends the base service) for `LoggerService`, which is what is going to use it as a provider for the current component and its children.

```ts
@Injectable({
  providedIn: 'root'
})
export class DateLoggerService extends LoggerService // <---
{
  override logInfo(msg: any)  { super.logInfo(stamp(msg)); }
  override logDebug(msg: any) { super.logInfo(stamp(msg)); }
  override logError(msg: any) { super.logError(stamp(msg)); }
}

function stamp(msg: any) { return msg + ' at ' + new Date(); }
```

### `useExisting`
This lets you map one token to another, creating two ways to access the same service object.

```ts
{ provide: MinimalLogger, useExisting: LoggerService },
```

Imagine the `LoggerService` has a massive [[API]] and you only wantto provide access to a shrinked version of it. In this example `MinimalLogger` is a **class-interface** and it reduces the API to two members.

```ts
// Class used as a "narrowing" interface that exposes a minimal logger
// Other members of the actual implementation are invisible
export abstract class MinimalLogger {
  abstract logs: string[];
  abstract logInfo: (msg: string) => void;
}
```

Since when you inject a `MinimalLogger` you need to use that type, you only have access to the API defined there. This is only thanks to [[TypeScript]], since behind the scenes [[Angular]] still provides the full `LoggerService`.

### `useFactory`
This lets you create a dependency object by calling a factory function, whose inputs  can be a combination of *injected services* and *local state*.

```ts
{ provide: RUNNERS_UP,    useFactory:  runnersUpFactory(2), deps: [Hero, HeroService] }
```

Note that this form of provider also takes in a third key: `deps`, which specifies dependencies for the `useFactory` function.

```ts
export function runnersUpFactory(take: number) {
  return (winner: Hero, heroService: HeroService): string =>
    /* ... */
}
```

## Class interface
TODO...

## Resources
https://angular.io/guide/dependency-injection-in-action