Tags: #angular #frontend #testing 
Created: 2023-02-07 15:02
References: https://github.com/ngneat/spectator#installation

# Angular Spectator
Spectator is a library that makes testing in [[Angular]] a much simpler task by removing a lot of the boilerplate code required.

First thing you need is a component factory. You can get one by using `createComponentFactory(Component)`. This function returns another function that you can then use to create the component you want to test before each of the tests.

```ts
describe('ButtonComponent', () => {
  let spectator: Spectator<ButtonComponent>;
  const createComponent = createComponentFactory(ButtonComponent);

  beforeEach(() => spectator = createComponent());

  it('should have a success class by default', () => {
    expect(spectator.query('button')).toHaveClass('success');
  });

  it('should set the class name according to the [className] input', () => {
    spectator.setInput('className', 'danger');
    expect(spectator.query('button')).toHaveClass('danger');
    expect(spectator.query('button')).not.toHaveClass('success');
  });
});
```

The `createComponentFactory` can optionally take much more config options that can be used to describe a test module configuration and much more.

The `createComponent` function can also take more parameters that can be used to specify its inputs, outputs and other things. This function returns an instance of `Spector` which exposes the following API:
- `fixture` - the tested component's fixture
- `component` - the tested component's instance
- `element` - the tested element's native element
- `debugElement` - the tested fixture's debug element
- `inject()` - a wrapper for `TestBed.inject()`
- many more useful things that can be used in tests

For events, you can use the `Spectator` object to dispatch events. Some examples for this would be:

```ts
// click
spectator.click(SpectatorElement);
spectator.click(byText('Element'));

// custom event
spectator.triggerEventHandler(MyChildComponent, 'myCustomEvent', 'eventValue');
spectator.triggerEventHandler(MyChildComponent, 'myCustomEvent', 'eventValue', { root: true});
```

For selecting elements, you can use many types of selectors, one example of that is:

```ts
// Returns a single HTMLElement
spectator.query('div > ul.nav li:first-child');
// Returns an array of all matching HTMLElements
spectator.queryAll('div > ul.nav li');

// Query from the document context
spectator.query('div', { root: true });

spectator.query('app-child', { read: ChildServiceService });
```

In case you want to mock components, you can use the `ng-mocks` library for that:

```ts
import { createHostFactory } from '@ngneat/spectator';
import { MockComponent } from 'ng-mocks';
import { FooComponent } from './path/to/foo.component';

const createHost = createHostFactory({
  component: YourComponentToTest,
  declarations: [
    MockComponent(FooComponent)
  ]
});
```

This library can also be used to test Angular services, directives, mock the routing module, mock the HTTP client module and much more.