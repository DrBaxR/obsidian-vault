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
TODO...

## Resources
https://angular.io/guide/dependency-injection-overview
