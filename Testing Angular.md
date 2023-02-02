Tags: #reference #testing #angular #frontend 
Created: 2023-02-02 12:02

# Testing Angular
This note includes a summary of the more important parts of the book that can be found in the *resources section*.

## Testing principles
Here are some advantages of testing your code:
1. **Testing saves time and money**: This happens because tests prevent bugs from causing real damage.
2. **Testing dormalizes and documetns requirements**: Tests are a formal human and machine readable way to describe how the code should behave.
3. **Testing make changes safe by preventing regressions**: Tests don't only prevent issues in the present but also in the future, when refactofing on modifying old code.

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



## Resources
https://testing-angular.com/target-audience/#target-audience