Tags: #reference 
Created: 2022-09-19 20:09

# Angular Dependency Injection
[[Angular]] supports the [[Dependency Injection]] design pattern, which is generally applied for [[Angular Services]].

Even though dependencies are generally services, they can also be values such as strungs or functions. The injector of an application instantiates dependencies when needed.

## Understanding dependency injection
Two main roles exist in the DI system: *dependency consumer* and *dependency provider*. The thing that connects the two roles is called the **injector**.

When a dependency is requested, the injector checks its registry to see if there is an instance already available in there. If there is not, a new instance is created and added to the registry.

### Providing a dependency
The first step is to add the `@Injectable` decorator on the class that you want to inject (in the case of services).

Next up, you need to make it available in the DI by providing it. There are multiple ways you can do that:

1. At component level, using the `providers` of `@Component`, making it available only for the component:

```ts
@Component({
  selector: 'hero-list',
  template: '...',
  providers: [HeroService]
})
class HeroListComponent {}
```

When you register a provider at component level, a new isntance of that provider gets created for each component instance.

2. At module level, using the `providers` of the `@NgModule` decorator, which makes the service available to all the module's consumers:

```ts
@NgModule({
  declarations: [HeroListComponent]
  providers: [HeroService]
})
class HeroListModule {}
```

This makes the same instance of the service available to all the components in that module.

3. At the application root level, which allows the service to be injected anywhere in the application:

```ts
@Injectable({
  providedIn: 'root'
})
class HeroService {}
```

This makes the service be a [[Singleton]].

### Injecting a dependency
The most common way to inject a service is by declaring it as a parameter of the constructor. 

When Angular discovers that a component depends on  aservice, it first checks if the injector has any existing instances of that service. If it doesn't, the injector creates one using the registered provider, adds it to the injector and then it gives it to the component.

## Creating an injectable service
*An [[Angular Service]] is typically a class with a narrow, well defined purpose.*

Angular distunguishes components from services to increase modularity. By separating a component's view-related features from other kind sof processing you can make you components lean and efficient.

*Ideally, a component's job is to enable the user experience and nothing more.* Its methods should mediate between the **view** and the **application logic**.

## Configuring dependency providers
This describes how you can use **classes** as dependencies. Besides classes, other things can also be provided as dependencies, such as *booleans*, *strings*, *dates* and *objects*.

You can make the provider use the same class as the one you specified or you can use another class. For example:

```ts
[{ provide: Logger, useClass: Logger }]
```

There are a few notable things here: 
- The `provide` property holds the token that is used for locating the dependency and configuring the injector
- The second property tells the injector *how* to create the dependency value. The provider definition can be one of the following:
	- `useClass` - alias a token and reference an existing one
	- `useFactory` - define a function that constructs the dependency
	- `useValue` - static value that will be used as a dependency

### Using an `InjectionToken` object
These are used for non-class dependencies.

```ts
import { InjectionToken } from '@angular/core';

export const APP_CONFIG = new InjectionToken<AppConfig>('app.config');
```

After doing this, you need to register the dependency provider in the component using the object of `APP_CONFIG`.

```ts
providers: [{ provide: APP_CONFIG, useValue: HERO_DI_CONFIG }]
```

After doing all this, you can inject them by using the `@Inject` deconrator in the constructor:

```ts
constructor(@Inject(APP_CONFIG) config: AppConfig) {
  this.title = config.title;
}
```

All this is required because it's not possible to use interfaces as providers, because of the way typescript compiles into javascript.

## Hierarchical injectors
These things are used to *set* the desired visibility of your provider. Angular has two injector hierarchies:
- `ModuleInjector` - you configure it in a hierarchy using `@NgModule` or `@Injectable`
- `ElementInjector` - implicitly created at each [[DOM]] element

### `ModuleInjector`
It can be configured in two ways: the first one is using the `@Injectable` `providedIn` property to refer to a `@NgModule` or `root`; the other way is the `@NgModule` `providers` array.

**Note:** The prefered way of configuration is the first one, since optimization tools can perform tree shaking.

The whole DI mechanism's providers are given by a hierarchy of *injectors*. This hierarchy starts at a `NullInjector`, which always throws an error unless `@Optional` was used. The next one in the hierarchy is the `ModuleInjector`, which contains platform specific things like `DomSanitizer`. The final pre-determined injector is the `root`, which is configured by your `AppModule`.

All dependency requests fowward up to the `root` injector (in the injector hierarchy).

### `ElementInjector`
This injector is created implicitly for each DOM element and you can provide services using it by specifying it in the `@Component` decorator using its `providers` property.

The element injectors also form a tree, which means that child components will also see dependencies provided in the element injector of their parents.

Note: A component is a special kind of [[Directive]], which means that they both have a `providers` property. Components and directives on the same element share an injector.

## Resolution rules
When a component declares a dependency, Angular first checks the `ElementInjector` hierarchy starting at the injector of the place where the dependency was requested and going up.

If no provider is found in the `ElementInjector` tree, then Angular starts looking in the `ModuleInjector` tree. Not finding a dependency in the module tree either results in an error being thrown.

## Resolution modifiers
Angular's resolution behaviour can be modified with `@Optional`, `@Self`, `@SkipSelf`and `@Host`.

There is a difference between the component decorator `providers` and `viewProviders`. The providers are applied in the component while view providers are applied in the component's *view*. For more details check the *logical structure of the template* section in the article.

A great example where this difference is illustrated is when you apply `@Host` and `@SkipSelf` to the same injected dependency.

## Examples
Some great examples that illustrate how the whole mechanism works start at [here](https://angular.io/guide/hierarchical-dependency-injection#logical-structure-of-the-template).

## Resources
https://angular.io/guide/dependency-injection-overview
