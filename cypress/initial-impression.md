# Crash Courses and misc videos

## File structure

- fixtures: as the name suggests, it stores dummy data and "fixed" stuff in order to be used by the suite.
- integration: where the folders and its subsequest test files are grouped into suites of independent test.
- plugins: extends the functionality of cypress.

## Inside a suite

The looks are very similar to other testing frameworks such as jest, but the main idea behind cypress is to be the browser itself. This (being part of the browser and not a hidden dom) allows for otherwise not possible end to end testing, and integration testing as if you are running the tests manually.

Each main testng is described with a `describe` along with an `it` that describes each suite that is usually independant. Testing operations api starts with `cy` and is followed by the method we want such as `cy.visit(url-string)` that sends the test to the intended URL.

Another is `cy.contains(text)`, it helps find text in the current screen with `cy`.

`cy.get('selector')` takes the selector and matches it with the current DOM, returning the elements in a way very similar to `querySelectorAll(selector)`. But to access inner content is to use `.should(condition, value)`, where the condition looks like `.should('have.text', 'This value')`.

As you have seen so far, no `let`, `var` or `const` are used to do any operations, and instead operations are dot chained to do the next related opration.

more `cy` operations are `cy.viewport()` helps make sure responsiveness are taken into consideration. (Will need to check if they actually change deviceType).


