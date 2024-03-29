Tags: #reference #testing #angular #frontend 
Created: 2023-02-02 12:02

# Testing Angular
This note includes a summary of the more important parts of the book that can be found in the *resources section*.

## Testing principles
Here are some advantages of testing your code:
1. **Testing saves time and money**: This happens because tests prevent bugs from causing real damage.
2. **Testing normalizes and documenta requirements**: Tests are a formal human and machine readable way to describe how the code should behave.
3. **Testing make changes safe by preventing regressions**: Tests don't only prevent issues in the present but also in the future, when refactoring on modifying old code.

The International Software Testing Qualifications Board (ISTQB) came up with a couple of principles that shed light on what you can achieve with tests.

### Discover Bugs
The purpose of a test is to *discover bugs*. If a test fails, it proves the presence of a bug. If the test passes, it proves that this particular test setup did not trigger a bug. **It does not prove that the code is correct or free of bugs**.

### Test high-risk cases
Should you write tests for all possible cases? **No**. First off, is is not cost effective, and second off, it will just give you a false sense of security, because no software is perfect.

### Adapt testing approach
It is important to *adapt tests based on context*. There are multiple classes of bugs that can pop up in an application and specific types of tests that prevent them. You need to try different approaches to fund new classes of bugs.

### The right amount of testing
There is a fierce debate that revolves around the right amount of testing. Too little leads to bugs, but too much consumes development time.

A sweet spot needs to be reached: *If testing practice deteriorates from this point, you run into more problems; and if you add more tests you observe little benefit.*

Tests differ in quality and meaningfullness. If **important** tests fail, the application is actually unusable. This means that **the quality of tests is more important thant their quantity**.

### Comparison between test types
*E2E tests:*
 - Advantages:
	- high benefits (they indicate exactly how the application works for the user)
 - Disadvantages:
	 - expensive and slow
	 - unreliable (hard to debug)
	 - high setup cost

*Unit tests:*
 - Advantages:
	  - precise and cheap
 - Disadvantages:
	  - too low level to check if feature works for end user
	  - may break and require fixing because of technical reasons (eg. you refactor something), not because something broke

*Integration tests:*
 - Advantages:
	 - experts recommend that most testing effort is spent on this level
	 - a good balance of all advantages and disadvantages of previous testing types

### Black box testing vs. White box testing
Black box testing only tests the outputs based on certain inputs given, while white box testing checks on the internals of the thing that is being tested.

Good practice: **Write black box tests whenever possible.**

## Angular testing principles
Angular is a framework that is opinionated and comes with a couple of architectural choices that make the parts of the applications that you build with it easily testable.

### Dependency injection and faking
**Dependency injection** and the underlying **inversion of control** turns tight coupling into loose coupling: A certain application part no longer depends on a specific class, function or object, it merely depends on a **token** that can be traded for a concrete implementation.

The reason why this is really important is because by taking this approach it's possible to deal with a part of the application in one of two ways in our tests:
1. provide an **original** and fully functional implementation, in which case we are writing an *integration test*
2. provice a **fake** implementation that does not haveside effects, in which case we are writing a *unit test*

### Testing tools
[[Angular]] uses by default [[Jasmine]] as the testing framework and [[Karma]] as the test runner. These tools can be replaced for alternatives, such as Jest.

### Running the unit and integration tests
The unit and integration tests of the project can be run by using the `ng test` command.

The `*.spec.ts` files each contain atleast one Jasmine test suite.

## Test suites with Jasmine
Jarmine consists of three important parts:
1. a library with classes and function for constructing tests
2. a test execution engine
3. a reporting engine that outputs test results in different formats

### Creating a test suite
A test suite can be created using a `describe` block, blocks which can be nested:

```ts
describe('Suite description', () => {
  describe('One aspect', () => {
    /* … */
  });
  describe('Another aspect', () => {
    /* … */
  });
});
```

### Specifications
Each suit is made out of more *specifications*, or specs. A spec is declared with an `it` block:

```ts
describe('Suite description', () => {
  it('Spec description', () => {
    /* … */
  });
  /* … more specs …  */
});
```

The pronoun `it` refers to the code under test. *It* should be the subject to a human-readable sentence, so that any stakeholder or team member that does not know JavaScript can read the code and understand what it does.

**Note:** Don't use the word 'should' when describing what the test does, since it's redundant.

### Structure of a test
Inside the `it` lies the actual test code. Indifferently of the test framework, test code typically consists of three phases:
1. **Arrange:** preparation and setup phase. Dependencies are set up. Spies and fakes are created.
2. **Act:** interraction with the code under test happens. A method is called, a HTML element is clicked etc.
3. **Assert:** code behaviour is checked and verified. The actual output is compared with the expected output.

### Expectations
To write a Jasmine spec, you can create expectations with the `expect` function together with a **matcher**.

```ts
const expectedValue = 5;
const actualValue = add(2, 3);
expect(actualValue).toBe(expectedValue);
```

### Efficient test suites
When you write multiple specs in one suite, you quickly realize that the *Arrange* phase is similar or even identical across these specs. For this reason `beforeEach`, `beforeAll`, `afterEach` and `afterAll`.

```ts
describe('Suite description', () => {
  beforeAll(() => {
    console.log('Called before all specs are run');
  });
  afterAll(() => {
    console.log('Called after all specs are run');
  });

  beforeEach(() => {
    console.log('Called before each spec is run');
  });
  afterEach(() => {
    console.log('Called after each spec is run');
  });

  it('Spec 1', () => {
    console.log('Spec 1');
  });
  it('Spec 2', () => {
    console.log('Spec 2');
  });
});
```

## Faking dependencies
The unit tests replace the dependencies with fakes in order to isolates code under test.

These replacements are also called *test doubles*, *stubs* and *mocks*. Replacing a dependency is called *stubbing* or *mocking*.

### Equivalende of fake and originals
A fake implementation must have the shape of the original. The fake does not need to be complete, but sufficient  enough to act as a replacement.

The biggest danger when faking a orginal is creating a fake that does not properly mimic the original. Also even if the fake resembles the original at the time of writing the code, it might easily get out of sync later, when the original is changed (the API changes or something like that).

The way to make sure none of that happens is to use [[TypeScript]] and strict typing, since the compiler will help us with that.

### Faking functions with Jasmine spies
Jasmine spies are a powerful mechanism with which we can mock functions and later check how the functions were called (with what parameters, how many times etc.).

For example, if we have a `TodoService` that expects a `fetch` function to be injected, which normally would make an API call, when testing the service we can mock that function with a spy:

```ts
class TodoService {
  constructor(
    // Bind `fetch` to `window` to ensure that `window` is the `this` context
    private fetch = window.fetch.bind(window)
  ) {}

  public async getTodos(): Promise<string[]> {
    const response = await this.fetch('/todos');
    if (!response.ok) {
      throw new Error(
        `HTTP error: ${response.status} ${response.statusText}`
      );
    }
    return await response.json();
  }
}
```

```ts
// Fake todos and response object
const todos = [
  'shop groceries',
  'mow the lawn',
  'take the cat to the vet'
];
const okResponse = new Response(JSON.stringify(todos), {
  status: 200,
  statusText: 'OK',
});

describe('TodoService', () => {
  it('gets the to-dos', async () => {
    // Arrange
    const fetchSpy = jasmine.createSpy('fetch')
      .and.returnValue(okResponse);
    const todoService = new TodoService(fetchSpy);

    // Act
    const actualTodos = await todoService.getTodos();

    // Assert
    expect(actualTodos).toEqual(todos);
    expect(fetchSpy).toHaveBeenCalledWith('/todos');
  });
});
```

### Spying on existing methods
Let's say that the `TodoService` does not use dependency injection to receive a fetch function. It is possible to tell Jasmine to replace the `window.fetch` function with a fake for the test and attach a spy to it, by using the `spyOn`, function:

```ts
// Fake todos and response object
const todos = [
  'shop groceries',
  'mow the lawn',
  'take the cat to the vet'
];
const okResponse = new Response(JSON.stringify(todos), {
  status: 200,
  statusText: 'OK',
});

describe('TodoService', () => {
  it('gets the to-dos', async () => {
    // Arrange
    spyOn(window, 'fetch')
      .and.returnValue(okResponse);
    const todoService = new TodoService();

    // Act
    const actualTodos = await todoService.getTodos();

    // Assert
    expect(actualTodos).toEqual(todos);
    expect(window.fetch).toHaveBeenCalledWith('/todos');
  });
});
```

## Debugging tests
Writing tests is exactly like writing normal code. In other words, you will need to debug them, because they will fail/pass when they shouldn't.

### Test focus
It is possible for some tests to take a really long time to run. By default, everytime you save a change to any test, Karma and Jasmine recompile and rerun all specs, which can get really annoying if you only need to see the result of a single test and you have to wait for all other tests to run.

To avoid this, you can **focus** on a single suite, which makes it so that only that suite is run. To do that, use `fdescribe`:

```ts
fdescribe('Example spec', () => {
  it('one spec', () => { /* … */ });
  it('another spec', () => { /* … */ });
});
```

You can also use `fit` to focus on a single spec:

```ts
describe('Example spec', () => {
  fit('one spec', () => { /* … */ });
  it('another spec', () => { /* … */ });
});
```

### Developer tools
The Jasmine test runner is just a simple web page, which means that you can use the usual dev tools of the browser.

## Testing components
This section describes the process of testing an Angular component.

### TestBed
Rendering a component in Angular requires several steps: templates need to be translated to JavaScript, an instance of the component needs to be created, dependencies need to be created and injected and inputs need to be set.

It is possible to do all of this manually, however for that you would need to be pretty deep into Angular's guts, so the Angular team provides a `TestBed` to ease unit testing.

### Configuring the testing module
The `TestBed` comes with a module that is created and configured like any other Angular module: you can declare components, directives, pipes, provide services etc.

For a unit test, just add the parts that are strictly necessary: the code under test, mandatory dependencies and fakes:

```ts
TestBed.configureTestingModule({
  declarations: [CounterComponent],
});
```

After this, the declared module and its things need to be compiled, which can be chained:

```ts
TestBed
  .configureTestingModule({
    declarations: [CounterComponent],
  })
  .compileComponents();
```

### Rendering the component
After the test bed is configured, we can render the component under test:

```ts
const fixture = TestBed.createComponent(CounterComponent);
```

This returns a `ComponentFixture`, which is a wrapper around the component with useful testing tools. After being done with that, all the static elements are rendered, however the dynamic parts, such as template bindings are not.

The reason is because there is no **automatic change detection**. It needs to be triggered manually.

```ts
fixture.detectChanges();
```

### TestBed and Jasmine
The code for rendering a coponent using the `TestBed` is now complete. Here is how you can load it in a Jasmine test suite:

```ts
describe('CounterComponent', () => {
  let fixture: ComponentFixture<CounterComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [CounterComponent],
    }).compileComponents();

    fixture = TestBed.createComponent(CounterComponent);
    fixture.detectChanges();
  });

  it('…', () => {
    /* … */
  });
});
```

The `compileComponents` method is async and therefore you need to `await` for it do be done.

### ComponentFixture and DebugElement
`ComponentFixture` contains the component and provides a convenient interface to both the component instance and the rendered [[DOM]] element.

You can access the component in case you need to set inputs/subscribe to outputs:

```ts
// This is a ComponentFixture<CounterComponent>
const component = fixture.componentInstance;
// Set Input
component.startCount = 10;
// Subscribe to Output
component.countChange.subscribe((count) => {
  /* … */
});
```

In case you need access to the DOM element, here is how you can access it:

```ts
const { debugElement } = fixture;
const { nativeElement } = debugElement;
console.log(nativeElement.tagName);
console.log(nativeElement.textContent);
console.log(nativeElement.innerHTML);
```

### Querying the DOM with test ids
Every `DebugElement` has the methods `query` and `queryAll` for finding descendant elements:

```ts
const { debugElement } = fixture;
// Find the first h1 element
const h1 = debugElement.query(By.css('h1'));
// Find all elements with the class .user
const userElements = debugElement.queryAll(By.css('.user'));
```

Since querying by tag names and class name leads to *tight coupling* between the test code and the template of the component, it's better to use **test ids** when querying for elements. Test ids are identifiers that are given to an element just for the purpose of finding it in a test.

```html
<button (click)="increment()" data-testid="increment-button">+</button>
```

```ts
const incrementButton = debugElement.query(
  By.css('[data-testid="increment-button"]')
);
```

### Triggering event handlers
In case you want to trigger events on elements, you can use `triggerEventHandler`:

```ts
incrementButton.triggerEventHandler('click', {
  /* … Event properties … */
});
```

### Expecting text output
Let's say that we want to check that clicking the increment button in a stepper component will increase the displayed number. Here is how that would work:

```ts
describe('CounterComponent', () => {
  let fixture: ComponentFixture<CounterComponent>;
  let debugElement: DebugElement;

  // Arrange
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [CounterComponent],
    }).compileComponents();

    fixture = TestBed.createComponent(CounterComponent);
    fixture.detectChanges();
    debugElement = fixture.debugElement;
  });

  it('increments the count', () => {
    // Act
    const incrementButton = debugElement.query(
      By.css('[data-testid="increment-button"]')
    );
    incrementButton.triggerEventHandler('click', null);
    // Re-render the Component
    fixture.detectChanges();

    // Assert
    const countOutput = debugElement.query(
      By.css('[data-testid="count"]')
    );
    expect(countOutput.nativeElement.textContent).toBe('1');
  });
});
```

Note the manual change detection trigger after triggering the click event.

### Testing helpers
It goes without saying that the testing code may get repetitive, and therefore over time you will also develop a suite of testing helpers, which are functions that result after refactoring your tests.

### Filling out forms
Filling out forms is different based on the input type and value. The generic answer is **find the native DOM element and set the `value` property to the new value**.

Only doing that however does not fully simulate a user's input and will not work with template-driven and reactive forms. For compatibility with Angular we need to dispatch a fake `input` event.

```ts
const resetInputEl = findEl(fixture, 'reset-input').nativeElement;
resetInputEl.value = '123';
resetInputEl.dispatchEvent(new Event('input'));
```

### Testing inputs
Inputs can be set in tests via the `fixture.componentInstance` field:

```ts
const component = fixture.componentInstance;
component.startCount = 10;
```

Note that `ngOnInit` and the other life cycle hooks do not get called automatically (just like the change detection), which means that we need to call them by hand:

```ts
    component.startCount = startCount;
    // Call ngOnChanges, then re-render
	component.ngOnChanges();
	fixture.detectChanges();
```

**Note:** This is what the book says, in what I tested there is no need to manually call the life cycle hook, because detecting changes was enough.

### Repetitive component specs
Experts disagree on whether repetitive testing code is a problem at all. 

**On one hand**, testing helpers form a custom language that make tests briefer and easier to understand. 

**One the other hand**, abstractions make tests more complex and therefore harder to understand, which is an issue because a developer reading the specs needs to be familiar with the testing helpers first (*tests should be more readable than the implementation code*).

### Black vs. White box component testing
Note that in Angular internal properties are not necessarily `private`, since they need to be `public`, in order to be accessed from the template. This happened up until Angular 14 (or 15, I don't remember), when you can access `protected` properties from the template.

This is an advantage because it enforces black-box testing, which is preferred to white-box testing. It enforces it because now you can't access the internals of the component from the test.

## Testing components with children
The component that was given as example was a component that directly renders HTML and does not use any other child components. This type of component is called a **presentational component**, which are considered low-level component and are easy to manage.

On the other side, there are components that bring multiple low-level components and pull data from different sources, like services. These components are called **container components**. Container components have several types of dependencies, which makes them harder to test.

There are two ways to test components with children:
1. unit test using **shallow rendering**: child components are not rendered
2. integration test using **deep rendering**: child components are rendered.

### Shallow testing vs. Deep testing
We need to decide the level of testing detail for the nested components. If separate unit tests for them exist, we don't need to actually click on each respective increment button (counter example, again). **The integration test needs to prove that the four components work together, without going into the child component details**.

### Unit test
If we write a spec that looks like this:

```ts
describe('HomeComponent', () => {
  let fixture: ComponentFixture<HomeComponent>;
  let component: HomeComponent;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [HomeComponent],
    }).compileComponents();

    fixture = TestBed.createComponent(HomeComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('renders without errors', () => {
    expect(component).toBeTruthy();
  });
});
```

Then this will act as a *smoke test*. It checks the presence of a component instance, it does not assert anything specific of the component yet. It just proves that the component renders without errors.

However, this test **does not pass**. This is because Angular does not recognize the custom elements that appear in the template of the parent component (the ones with `app-*`). This is because no components are declared to match these selectors. There are two ways to fix this issue:
1. Declare the child components in the testing module. **This turns the test into an integration test**.
2. Tell Angular to ignore the unknown elements. **This turns the test into a unit test**.

If we want Angular to simply ignore the elements that it does not know, we can do something like this when declaring the module:

```ts
await TestBed.configureTestingModule({
  declarations: [HomeComponent],
  schemas: [NO_ERRORS_SCHEMA],
}).compileComponents();
```

Now the *smoke test passes*. To check that the independent counters render, we can do this:

```ts
it('renders an independent counter', () => {
  const { debugElement } = fixture;
  const counter = debugElement.query(By.css('app-counter'));
  expect(counter).toBeTruthy();
});
```

You can check that a component child of the parent component has rendered by doing this:

```ts
it('renders an independent counter', () => {
  const counter = findComponent(fixture, 'app-counter');
  expect(counter).toBeTruthy();
});
```

To test that an input received the proper value, you can access the `debugElement.properties` map of the component debug element:

```ts
it('passes a start count', () => {
  const counter = findComponent(fixture, 'app-counter');
  expect(counter.properties.startCount).toBe(5);
});
```

In case you need to trigger an event on one of the components that you have not rendered due to *shallow rendering*, you can do this:

```ts
it('listens for count changes', () => {
  spyOn(console, 'log');
  const counter = findComponent(fixture, 'app-counter');
  const count = 5;
  counter.triggerEventHandler('countChange', count);
  expect(console.log).toHaveBeenCalledWith(
    'countChange event from CounterComponent',
    count,
  );
});
```

Another thing that was showcased in the example above is the concept of *spying on functions*. With spies you can check how many times, with what parameters etc. a function has been called.

### Faking a child component
Instead of working with empty custom components, we can render **fake child components**. This is a little more useful than telling Angular to ignore all the elements it does not know, since it gives us more confidence when tests pass.

A fake component has the same interface (inputs, outputs, selector), but does not have any dependencies and does not render anything. Here is how you can reduce a component to an *empty shell*:

```ts
@Component({
  selector: 'app-counter',
  template: '',
})
class FakeCounterComponent implements Partial<CounterComponent> {
  @Input()
  public startCount = 0;

  @Output()
  public countChange = new EventEmitter<number>();
}
```

Now that fake component needs to be added to the declarations of the test module so that angular knows to use that component when it encounters the `app-counter` element. The advantage of using fake components is that we can now query using the `By.directive(...)` method, since components are also a kind of directive.

The advantage of this approach is that we can access the rendered component instance. Having access to the child component, we can make expectations against it.

Checking if the component is rendered/present:

```ts
it('renders an independent counter', () => {
  const counterEl = fixture.debugElement.query(
    By.directive(FakeCounterComponent)
  );
  const counter: CounterComponent = counterEl.componentInstance;

  expect(counter).toBeTruthy();
});
```

Checking the values of inputs:

```ts
it('passes a start count', () => {
  const counterEl = fixture.debugElement.query(
    By.directive(FakeCounterComponent)
  );
  const counter: CounterComponent = counterEl.componentInstance;

  expect(counter.startCount).toBe(5);
});
```

Output handling can be done by calling the `emit` method on the output that you want to check:

```ts
it('listens for count changes', () => {
  const counterEl = fixture.debugElement.query(
    By.directive(FakeCounterComponent)
  );
  const counter: CounterComponent = counterEl.componentInstance;

  spyOn(console, 'log');
  const count = 5;
  counter.countChange.emit(5);
  expect(console.log).toHaveBeenCalledWith(
    'countChange event from CounterComponent',
    count,
  );
});
```

### Faking a child component using ng-mocks
`ng-mocks` is a library that helps with mocking components. In the section above, we manually created the mock component. It's better (and less tedious) to use a library that does this for us.

If we use [ng-mocks](https://github.com/help-me-mom/ng-mocks), the only thing that changes in the previous code is declaring the component in the test module and querying for the component by the directive:

```ts
await TestBed.configureTestingModule({
      declarations: [HomeComponent, MockComponent(CounterComponent)],
      schemas: [NO_ERRORS_SCHEMA],
    }).compileComponents();
    
// ...

const counterEl = fixture.debugElement.query(
      // Original class!
      By.directive(CounterComponent)
);
counter = counterEl.componentInstance;
```

## Testing components that depend on services
The previous chapter covered testing components that don't rely on any services. In this chapter we will be using a component that injects a service that keeps track of the count.

There are two ways we can test this component:
1. A unit test that replaces the service it relies on (`CounterService`) dependency with a fake
2. An integration test that includes a real `CounterService`

### Service dependency integration test
For this specific case, since the component that we want to test does not use the service to do any HTTP calls or stuff like that, the test is pretty much the same with the other test we wrote a while back.

The only thing that changes is the fact that the test module needs to have the service specified as a provider:

```ts
providers: [CounterService],
```

### Faking service dependencies
Since the service we need is just a class decorated with `@Injectable`, we can make the fake be a simple object that contains the API that is used in the component (note the spies, no we can `expect` on the methods of the service implementation):

```ts
const currentCount = 123;

const fakeCounterService:
  Pick<CounterService, keyof CounterService> = {
  getCount:
    jasmine.createSpy('getCount').and.returnValue(of(currentCount)),
  increment: jasmine.createSpy('increment'),
  decrement: jasmine.createSpy('decrement'),
  reset: jasmine.createSpy('reset'),
};
```

After that, we can provide the fake to the component we are testing by using the following feature of Angular when declaring the test module:

```ts
providers: [
	{ provide: CounterService, useValue: fakeCounterService }
  ],
```

Other than these changes, we the test remains mostly the same.

### Fake service with minimal logic
In order to make the test scenario more realistic, we can also add the minimal logic to our fake service implementation. Here is how the implementation could look like:

```ts
fakeCount$ = new BehaviorSubject(0);

fakeCounterService = {
  getCount() {
	return fakeCount$;
  },
  increment() {
	fakeCount$.next(1);
  },
  decrement() {
	fakeCount$.next(-1);
  },
  reset() {
	fakeCount$.next(Number(newCount));
  },
};
spyOn(fakeCounterService, 'getCount').and.callThrough();
spyOn(fakeCounterService, 'increment').and.callThrough();
spyOn(fakeCounterService, 'decrement').and.callThrough();
spyOn(fakeCounterService, 'reset').and.callThrough();
```

Remember to call `.and.callThrough()` to make it so Jasmine also calls the original function. Otherwise it will just be ignored.

## Testing complex forms
Check the example presented in the book.

One powerful thing that you can use to simulate async validators (let's say that you use an async validator that uses a debounce time of 1 second, which means that you normally have to wait for 1 second to have the validator execute) is `fakeAsync`. You could use a `setTimeout(..., 1000)`, but that would slow down the test execution.

```ts
it('submits the form successfully', fakeAsync(async () => {
  await setup();

  fillForm();

  // Wait for async validators
  tick(1000);

  findEl(fixture, 'form').triggerEventHandler('submit', {});

  expect(signupService.signup).toHaveBeenCalledWith(signupData);
}));
```

Anytime the DOM changes, the change detection needs to be called (`fixture.detectChanges()`).

## Testing components with Spectator
The base Angular testing tools are a bit lacking: they require a lot a boiler-plate code, they make test specs get repetitive and they offer leaky abstractions (they make you use the thing they are abstracting directly).

For this, one alternative is to use [Spectator](https://github.com/ngneat/spectator). This section contains an overview of what you can do with this library.

### Component with an input
First off, you need a component factory. The `shallow` property refers to the rendering style, if you also need to render the child components or not:

```ts
const createComponent = createComponentFactory({
	component: FullPhotoComponent,
	shallow: true,
});
```

After you got the `createComponent`, you can obtain a `Spectator` object, which serves as an interface for all your testing needs:

```ts
beforeEach(() => {
	spectator = createComponent({ props: { photo: photo1 } });
});
```

Finally, you can write the assertions after querying by a test id (or by whatever else you'd like):

```ts
expect(
  spectator.query(byTestId('full-photo-title'))
).toHaveText(photo1.title);
```

### Component with children and service dependency
If you want to use mock components and providers, Spectator provides a neat way to create those (under the hood, it uses `ng-mocks`).

```ts
const createComponent = createComponentFactory({
	component: FlickrSearchComponent,
	shallow: true,
	declarations: [
	  MockComponents(
		SearchFormComponent, PhotoListComponent, FullPhotoComponent
	  ),
	],
	providers: [mockProvider(FlickrService)],
});
```

The `mockProvider` creates an object that resembles the original, but the methods are replaced with Jasmine spies.

After having the `createComponent`, we can jump to the `becoreEach` setup phase:

```ts
  beforeEach(() => {
    spectator = createComponent();

    spectator.inject(FlickrService)
	    .searchPublicPhotos.and.returnValue(of(photos));

    searchForm = spectator.query(SearchFormComponent);
    photoList = spectator.query(PhotoListComponent);
    fullPhoto = spectator.query(FullPhotoComponent);
  });
```

The `spectator.inject` method is equivalent to the `TestBed.inject` method. We get the `FlickrService` fake instance and configure the `searchPublicPhotos` method to return a fixed value.

Note that the `query` method can also find **components** by their classes and fetch them as component instances. Therefore, you can check the inputs via the component's instance object:

```ts
it('renders the search form and the photo list, not the full photo', () => {
  if (!(searchForm && photoList)) {
    throw new Error('searchForm or photoList not found');
  }
  expect(photoList.title).toBe('');
  expect(photoList.photos).toEqual([]);
  expect(fullPhoto).not.toExist();
});
```

Here is how you would test that a component's outputs are handled properly:

```ts
it('searches and passes the resulting photos to the photo list', () => {
  if (!(searchForm && photoList)) {
    throw new Error('searchForm or photoList not found');
  }
  const searchTerm = 'beautiful flowers';
  searchForm.search.emit(searchTerm);

  spectator.detectChanges();

  const flickrService = spectator.inject(FlickrService);
  expect(flickrService.searchPublicPhotos).toHaveBeenCalledWith(searchTerm);
  expect(photoList.title).toBe(searchTerm);
  expect(photoList.photos).toBe(photos);
});
```

### Event handling with Spectator
If you want, say, to click on an element you can do this:

```ts
describe('PhotoItemComponent with spectator', () => {
  /* … */

  it('focusses a photo on click', () => {
    let photo: Photo | undefined;

    spectator.component.focusPhoto.subscribe((otherPhoto: Photo) => {
      photo = otherPhoto;
    });

    spectator.click(byTestId('photo-item-link'));

    expect(photo).toBe(photo1);
  });

  /* … */
});
```

## Testing services
Services are used when you need to do some side effects or when components that are not parent and child need to communicate.

In theory, service is an umbrella term that refers to anything that can be injected as a dependency. The services that we want to test are that ones that are classes.

There are several types of service, but most of them follow one of these wide spread patterns:
- Services that have public methods that return values.
- Services that store data. They hold internal state. We can get/set the state.
- Services that interact with dependencies. These are often other services. For example, a service might send HTTP requests via Angular's `HttpClient`

### Testing a service with internal state
Let's say we have this service, that has some private state that can be modified via its public interface:

```ts
class CounterService {
  private count: number;
  private subject: BehaviorSubject<number>;
  public getCount(): Observable<number> { /* … */ }
  public increment(): void { /* … */ }
  public decrement(): void { /* … */ }
  public reset(newCount: number): void { /* … */ }
  private notify(): void { /* … */ }
}
```

First step is to identify what the service does and create a spec for each of the features:

```ts
describe('CounterService', () => {
  it('returns the count', () => { /* … */ });
  it('increments the count', () => { /* … */ });
  it('decrements the count', () => { /* … */ });
  it('resets the count', () => { /* … */ });
});
```

The arrange phase consists of instantiating the service (at the moment this is enough, since it is a simple service with no dependencies, but for more complex cases `TestBed` should be used):

```ts
describe('CounterService', () => {
  let counterService: CounterService;

  beforeEach(() => {
    counterService = new CounterService();
  });

  it('returns the count', () => { /* … */ });
  it('increments the count', () => { /* … */ });
  it('decrements the count', () => { /* … */ });
  it('resets the count', () => { /* … */ });
});
```

This is how the spec looks like for one of those features. It only tests the public members of the service, since it's not supposed to access the internal state of the service:

```ts
it('returns the count', () => {
  let actualCount: number | undefined;
  counterService.getCount().subscribe((count) => {
    actualCount = count;
  });
  expect(actualCount).toBe(0);
});
```

Keep in mind that within the test environment the observable's callback is called synchronously, when it's needed.

### Testing a service that sends HTTP requests
Let's say that we have a `FlickrService` that sends some HTTP requests by using Angular's `HttpClient`. There are two ways to test this service:
- an integration test
- a unit test

For an **integration** test you need to inject the actual `HttpClient` and make real HTTP requests. It makes little sense to make requests to a production third party API in a testing environment. It makes more sense to use a dedicated test API that returns a fixed value. This API can run on the same machine or in the local network.

In this specific it's better to write a **unit test**. Angular has a powerful helper for testing code that depends on `HttpClient`: the `HttpClientTestingModule`.

For testing a service with dependencies it's really tedious to instantiate a service with `new`. For that we use the `TestBed.inject` method.

#### Call the method under test
First off, we call the method that we want to test and subscribe to the observable it returns:

```ts
const searchTerm = 'dragonfly';

describe('FlickrService', () => {
  let flickrService: FlickrService;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [FlickrService],
    });
    flickrService = TestBed.inject(FlickrService);
  });

  it('searches for public photos', () => {
    flickrService.searchPublicPhotos(searchTerm).subscribe(
      (actualPhotos) => {
        /* … */
      }
    );
    /* … */
  });
});
```

#### Find pending requests
In the second step we need to find the pending request using the `HttpTestingController`, which is a service from the HTTP testing module we added as a dependency:

```ts
controller = TestBed.inject(HttpTestingController);
```

Now, with the `controller` you can see what pending requests there are:

```ts
const request = controller.expectOne(expectedUrl);
```

#### Respond with fake data
After we have a pending request that has been checked with `expectOne`, we can send a fake response to that request that the service will receive via the `request` that we cot from the `controller`.

```ts
request.flush({ photos: { photo: photos } });
```

#### Check the result of the method called
Now that we received a request and send a specific response, we can verify the response received by the service:

```ts
let actualPhotos: Photo[] | undefined;
flickrService.searchPublicPhotos(searchTerm).subscribe(
  (otherPhotos) => {
    actualPhotos = otherPhotos;
  }
);

const request = controller.expectOne(expectedUrl);
// Answer the request so the Observable emits a value.
request.flush({ photos: { photo: photos } });

// Now verify emitted valued.
expect(actualPhotos).toEqual(photos);
```

#### Verify that all the requests have been answered
This can be done by calling `conroller.verify()`. This assures us that the service did not make any other requests besides the one that we expected.

#### Testing the error case
In case you want to test what would happen if the response of a request is an error request, you can do this:

```ts
const status = 500;
const statusText = 'Internal Server Error';
const errorEvent = new ErrorEvent('API error');
/* … */
const request = controller.expectOne(expectedUrl);
request.error(
  errorEvent,
  { status, statusText }
);
```

**Note:** The API of the `controller` gives us way more options. In case you need those, you can check its documentation.

## Resources
https://testing-angular.com/target-audience/#target-audience