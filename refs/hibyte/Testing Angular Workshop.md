Tags: #reference 
Created: 2023-02-07 11:02

# Testing Angular Workshop

## Why is testing important?
The main reasons why writing automated tests is important in any type of software are:
1. **It saves time**: You test your application either way, the only thing is that you are doing it *manually*. By writing automated tests you can simply press a button and you run all your tests.
2. **It makes refactoring a safer activity**: By having a suite of automated tests you can simply run anytime and you trust that if they pass then the application is working correctly, you can fearlessly refactor code without worrying that you may have broken something with that refactoring.
3. **It documents code**: By writing tests for your code you are also explicitly describing what your code is doing, which is a great help for future developers.
4. **It enforces good practices**: Tests make you write code that is isolated and can be easily transferred into another environment, other than the production, which means that the code is reusable, which is a good thing.

## Types of tests
There are three main types of tests:
1. **Unit tests**: These are the tests that make sure that the components of your application are working in an isolated environment. It is common that if the component you are testing depends on another part of the application you replace that original dependency with a *fake* (or *mock*) of that dependency, that does close to nothing.
2. **Integration tests**: These are tests that make sure that multiple components (or units) of your application work together, as a sub-system of the whole application. In the case of integration tests, the dependencies are no longer mocked, since you want to make sure that components work well together with things that they depend on.
3. **End-to-End tests**: These are the tests that cover your whole application (from front-END TO back-END, hence the name - *END TO END*). These tests are usually run in a browser that is instructed what to do by the code of the tests you write.

Neither of these test types are perfect: they all have their advantages and disadvantages. Therefore, it's a good idea to integrate all of them in your projects.

## Unit test for `TodoCardComponent`
You need to import the `TestBed` from `@angular/core/testing`. After that you need to create a testing module, compile it and finally create the `TodoCard` component . All this can be done in the `beforeEach` block:

```ts
beforeEach(async () => {
	await TestBed.configureTestingModule({
		declarations: [TodoCardComponent],
	}).compileComponents();
	
	fixture = TestBed.createComponent(TodoCardComponent);
});

it('mounts', () => {
	expect(fixture).toBeTruthy();
});
```

Decide what are the main features of the component you are testing and write a spec for each of them:

```ts
it('properly renders its input todo', () => {
	/* ... */
});

it('emits delete event when "Delete" button clicked', () => {
	/* ... */
})

it('emits edit event when "Edit" button clicked', () => {
	/* ... */
})
```

In order to access a component's `@Input`s and `@Output`s, you can use the `ComponentFixture<TodoCardComponent>` instance (which was set during the `beforeEach` phase):

```ts
fixture.componentInstance.todo = sampleTodo;
fixture.detectChanges();
```

Keep in mind that you need to **manually trigger the change detection mechanism**. You generally need to do this anytime you expect the DOM to change/re-render and you need to make `expect`s against it.

In order to find DOM elements, you can use the `debugElement.query` method from the fixture. With this, you can query for any element you want and by any criteria you want. It's generally a good practice to *query by a test id*, since classes, and the structure tends to change, but test ids don't.

```ts
const descriptionElement: HTMLElement = fixture
	.debugElement
	.query(By.css('[data-testid=description]'))
	.nativeElement;
```

In order to test component outputs, you can subscribe to the outputs (since `EventEmitter` just extends `Observable`), set a value in the subscription and then `expect` against that value:

```ts
let actualValue: number | undefined;
fixture.componentInstance.delete.subscribe(value => actualValue = value);

const debugElement = fixture.debugElement.query(By.css('[data-testid=delete]'));
debugElement.triggerEventHandler('click', new Event('click'));

expect(actualValue).toBe(sampleTodo.id);
```

**Note:** Keep in mind that the subscribe callbacks are called synchronously, when they happen within the tests, that is why the test can be written this way.

## Unit test for `TodoListComponent`
The `TodoListComponent` contains child components *and* it also injects a service as a dependency. There are two ways this type of component can be tested:
1. Use the real implementations of the child components and services it injects - this would make the test be an **integration** test.
2. Use mock/fake implementations for the child components and injected services - this makes the test be a **unit** test.

This note will cover the second case, where we mock the components and services used by the component.

If we set up the component in the exact same way we did with the component that had no children:

```ts
beforeEach(async () => {
	await TestBed.configureTestingModule({
		declarations: [TodoListComponent],
	}).compileComponents();
	
	fixture = TestBed.createComponent(TodoListComponent);
	fixture.componentInstance.todos = sampleTodos;
	fixture.detectChanges();
});

it('mounts', () => {
	expect(fixture).toBeTruthy();
});
```

Everything works well until we try to render the children elements (the ones with the selector `app-todo-card`). We can fix this issue by creating a mock of the `TodoCardComponent` and adding it to the `declarations` of the test module.

The mock component needs to have the same interface as the original component (same inputs and outputs):

```ts
@Component({
  selector: 'app-todo-card',
  template: '',
})
export class TodoCardMock {
  @Input() todo?: Todo;

  @Output() delete = new EventEmitter<number>()
  @Output() edit = new EventEmitter<number>()
}
```

In order to query for a child component, the `By.directive` predicate can be used. Also, in case you need multiple elements, you can use the `queryAll` method on the `debugElement`:

```ts
const todoCards = fixture
	.debugElement
	.queryAll(By.directive(TodoCardComponentMock));
```

After we have the mock component and we added it in the module, we can finally write the test that checks that the component renders a `TodoCardComponent` for each of the entries in the mock data we fed it:

```ts
it('renders a TodoCard for each of the data entries', () => {
	const todoCards = fixture
		.debugElement
		.queryAll(By.directive(TodoCardComponentMock));
	
	expect(todoCards.length).toBe(sampleTodos.length);
});
```

You can trigger events of the mock component via its `debugElement`, just like we triggered the `click` event in the `TodoCardComponent` test. Keep in mind that the second parameter of the `triggerEventHandler` method is what angular uses as `$event` in the template of components:

```ts
firstTodoComponent.triggerEventHandler('delete', idToDelete);
```

## Unit test with [Spectator](https://github.com/ngneat/spectator)
Spectator is a library that gets rid of a lot of the boilerplate code introduced by the base testing tools Angular offers and overall makes testing a better experience.

Another library that makes our testing much simpler is [ng-mocks](https://www.npmjs.com/package/ng-mocks). This one can be used to automatically mock whatever component we want.

Another thing that Jasmine lets you do is to only run the test/test suite that you are working on. This is really useful, since sometimes test take really long to run, which means that you'll have to wait a long time for the tests to compile/run.

## Integration test for `TodosComponent`
It does not really matter whether a test is a unit test or an integration test, as long as your application's tests cover as much of the application's logic.

The `TodosComponent` navigates to different routes. In order to write an integration test that checks whether the component navigates to the routes it's supposed to, we either need the use Angular's `RouterTestingModule` or Spectator's `createRoutingFactory`.

`createRoutingFactory` is an extension of `createComponentFactory` which includes some tools for testing components that use routing. In  case you need to check that some routes were reached within tests, you can set `stubsEnabled` to `false` and specify what routes the testing routing module should know of.

```ts
const createComponent = createRoutingFactory({
	component: TodosComponent,
	declarations: [TodoListComponent],
	providers: [TodosService, ThemeService],
	stubsEnabled: false,
	routes: [
		{ path: '', component: TodosComponent },
		{ path: 'edit/:id', component: TodoEditComponent },
		{ path: 'create', component: TodoCreateComponent },
	]
});
```

Later, when you make your `expect` statement you need to inject `Location`, which has a `path()` method that will give you the current URL:

```ts
expect(spectator.inject(Location).path()).toBe(`/edit/${editId}`);
```

## Unit test for `ThemeService`
Testing a service is much simpler than testing components, since they are just classes, and don't require any rendering.

All you need to test a service is to use the `createServiceFactory` Spectator function and then call `createService`:

```ts
let spectator: SpectatorService<ThemeService>;
const createService = createServiceFactory(ThemeService);

beforeEach(() => spectator = createService());

it('initially sets theme to light', () => {
	expect(spectator.service.dark).toBeFalse();
});

it('toggles theme', () => {
	spectator.service.toggleTheme();
	
	expect(spectator.service.dark).toBeTrue();
});
```

## Test services that inject `HttpClient`
In case you need to test a service that sends HTTP requests, you can use Angular's `HttpClientTestingModule` or Spectator's `createHttpFactory`.

When you use the function that `createHttpFactory` returns, it creates an instance of the `Spectator` class. This instance intercepts the HTTP requests, you can use it to check if a request has been sent and verify if any other unexpected requests have been sent.

```ts
const createService = createHttpFactory({
	service: ApiService,
	providers: [{ provide: BASE_URL, useValue: baseUrl }]
});

beforeEach(() => {
	spectator = createService();
});

it('sends DELETE request on delete with id', () => {
	const id = 1;
	spectator.service.delete(id).subscribe();
	
	spectator.expectOne(`${baseUrl}/todos/${id}`, HttpMethod.DELETE);
	spectator.controller.verify();
});
```

- `spectator.expectOne()` - asserts that a request of the type you specify has been sent and returns that request in case you need to make further assertions on it
- `spectator.controller.verify()` - uses the underlying controller object (provided by Angular's `HttpClientTestingModule`) to check whether there are any dangling requests

## End-to-End test using [Cypress](https://docs.cypress.io/guides/end-to-end-testing/writing-your-first-end-to-end-test)
Cypress is a framework that can be used to easily write e2e tests and run them. The first thing that you need to do is set up e2e testing in your project by using `ng e2e` and walking through the configuration phase.

After you're done with setting Cypress up, you can write your tests in the `./cypress/e2e` folder. Here is an example of how an end-to-end test looks like.

```ts
describe('Todo Items', () => {
		it('creates a todo item', () => {
		// setup
		cy.visit('http://localhost:4200');
		cy.get('app-todo-card').should('have.length', 3);
		cy.contains("+").click();
		
		// act
		cy.get('[data-testid=title-input]')
			.type('Example TODO');
		cy.get('[data-testid=due-date-input]')
			.type('2023-01-01');
		cy.get('[data-testid=description-input]')
			.type('Example TODO description');
		cy.get('[data-testid=submit-button]')
			.click();
		
		// assert
		cy.get('app-todo-card').should('have.length', 4);
	})
})
```

**Note:** `ng e2e` will run the tests against an instance of the front end that runs on your local machine. That's good, but a better approach would be to run the tests on an instance of the application that is deployed in a testing environment.

## Resources
[[Testing Angular]]
https://testing-angular.com
https://github.com/ngneat/spectator#testing-components
https://www.npmjs.com/package/ng-mocks
https://docs.cypress.io/guides/end-to-end-testing/writing-your-first-end-to-end-test