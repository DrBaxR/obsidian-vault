Tags: #reference #testing 
Created: 2023-01-30 14:01

# The Testing Pyramid
![testing pyramid](https://learn.cypress.io/images/testing-foundations/automated-tools-pyramid.png)

The testing pyramid illustrates the various types of tests and the relationships to each other.

## Unit tests ([[Unit Testing]])
These are the base of the pyramid and therefore they are the foundation of every other test and they are the most common tests that developers write.

These tests test a single *unit* of your application, which means that they should **not** be dependent upon other parts of the system/application. For example, think of testing a single function (rest of section will refer to testing functions, but unit tests are not necessarily always testing functions).

When writing unit tests you want to think of the functions you are testing as black boxes, you should not be concerned with the logic that the functions have inside them. *The only thing that you should be concerned with is that based on a specific input, the function should produce a specific ourput.*

## Integration tests
The purpose of [[Integration Testing]] is that the isolated parts (units) of your application work together as expected. In other words, they should test the dependencies within a system are working otgether correctly.

Integration tests are useful when you are building an application from scratch and you don't have an UI yet (you are testing pure logic). Once the UI is ready, you should start [[E2E Testing]].

## End to End tests
The purpose of these tests is to imitate what a real user would do. For example, you might write a single test that *registers a new account, logs in to that newly created account, purchases a product and then logs out.*

End to end (E2E) tests will often replace integration tests, since they are essentially testing the same thing.

## Resources
https://learn.cypress.io/testing-foundations/the-testing-pyramid