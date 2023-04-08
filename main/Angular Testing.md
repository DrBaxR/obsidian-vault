Tags: #frontend #angular #testing 
Created: 2023-01-31 12:01
References: 

# Angular Testing
This note contains information about what you can do in order to test you [[Angular]] application. It will cover [[Component Testing]] and [[E2E Testing]].

## Component testing
In the context of Angular web apps, component testing is equivalent to [[Unit Testing]], since they are both testing the smallest unit in an application in an isolated context.

Some good practices to keep your components easily testable are:
1. *Isolation:* Keep components isolated and only rely on inputs/outputs for communication with other components
2. *Small and focused:* Keep components small and make them focused on a single responsibility
3. *Avoid tight coupling:* Make components, services and modules as loosely coupled as possible
4. *Dependency injection:* Use dependency indection to make it possible to inject mock dependencies
5. *Clear API:* Expose a clear API via the component's inputs and outputs
6. *Avoid global state*: Don't use global state, such as `document` or `window`

## E2E testing
This consists in testing your application as a whole. It is recommended that the testing environment (the place where the tests are run) is deployed and similar to the production environment.

A good way to start E2E testing is to create some *user journeys* for the most critical places of your application. A user journey is a path that a user can take in your application (like *log in, navigate to the menus page and place an order*).

After having the user journeys, break them down into small steps and finally translate them into tests.