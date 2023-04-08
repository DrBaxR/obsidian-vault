Tags: #reference 
Created: 2023-01-30 14:01

# Identifying What to Test
If you are trying to test an already existing application, you can begin by testing your application's most *mission critical* (portions that cannot go down) pieces.

## User journeys
Now that the parts were identified, write the tests using *user journeys*. They are essential paths which a user of your application takes,

An entire user story should be a single end to end test, since it lets you know if all the pieces within your application are working correctly as a whole. Another advantage of testing user journeys is that this tests the application's whole tech stack with a single test.

## New Features
When implementing a new feature, a helpful technique for writing tests is to start with the end goal in mind and break it down in incremental steps, all of which can be translated into tests.

## Bugs
It is recomended to write tests around any bug that may appear in your application. A good approach is to write a failing test around the bug before fixing it. Once the bug has been fixed, the tests will pass.

## Resources
https://learn.cypress.io/testing-foundations/knowing-what-to-test