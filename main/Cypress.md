Tags: #frontend #angular #testing
Created: 2023-01-30 12:01
References: https://docs.cypress.io/guides/component-testing/angular/quickstart

# Cypress
Cypress is a library/framework that can be used to test [[Front End]] web applications. This note will cover how it can be used to test [[Angular]] components. It can be used for [[Component Testing]] and [[E2E Testing]].

## Installation
Cypress can be installed using `npm` and then you can run an `npx` script to open in and configure it as you wish.

The setup will take you through selecting a browser, installing dependencies and other steps.

## Component testing
For component testing, let's say that we have a counter component that has two buttons that increase/decrease the displayed counter.

When writing a test, you first need to `mount` the component that you wqant to test:

```ts
import { StepperComponent } from './stepper.component'

describe('StepperComponent', () => {
  it('mounts', () => {
    cy.mount(StepperComponent)
  })
})
```

The `describe` function describes a `spec` and it can contain multiple `it` statements. This allows us to nicely group tests.

The general flow of a test is to `get` a specific element *do some actions* and then make an *assertion*.

```ts
it('when the increment button is pressed, the counter is incremented', () => {  
	cy.mount(StepperComponent)  
	cy.get('[data-cy=increment]').click()  
	cy.get('[data-cy=counter]').should('have.text', '1')  
})
```

**Note:** The queries in the `get` method work just like [[CSS]] selectors. In the example above the query looks for an element that has an attribute set to a certain value:

```html
<span data-cy="counter">{{ count }}</span>
```

### Pass inputs to components
This can be done when mounting the component, via the `componentProperties`. This object's properties will be passed as inputs to the mounted component:

```ts
it('supports an "initial" prop to set the value', () => {
  cy.mount(StepperComponent, {
    componentProperties: {
      count: 100,
    },
  })
  cy.get('[data-cy=counter]').should('have.text', '100')
})
```

### Using spies
Spies can be used to validate the outputs of the component we are testing. Here is how spies can be used to make sure that the `change` event was fired when clicking a button:

```ts
it('clicking + fires a change event with the incremented value', () => {
  cy.mount(StepperComponent, {
    componentProperties: {
      change: createOutputSpy('changeSpy'),
    },
  })
  cy.get('[data-cy=increment]').click()
  cy.get('@changeSpy').should('have.been.calledWith', 1)
})
```
