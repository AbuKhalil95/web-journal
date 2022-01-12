# Cypress as a Mimic

Type in `/// <reference types="cypress" />` at the top of the file to get started with autocomplete support.

## Step 0 Collection

Main thing to consider when trying to build testing suits for any site, is to consider your selectors, just as if you would do it with plain JS. With `class`, `id`, `[attribute="value"]` and `class[attribute="value"]` selectors this won't be a problem, and its done at Cypress as `cy.get(selector)`.

Collect as much selectors as you need.

## Step 1 Visitation

Trigger a url with `cy.visit('url')`.

## Step 2 Pathing

Click on what is needed to be clicked with the method `.click()`, or clear a field with `.clear()` and then type in the value programmatically with `.type('2')`. Sometimes you might need to wait with .`wait(ms)` in order to wait for pages to load and continue testing.

## Step 3 Destination

Verify that a certain field has some value with `.contains(value)`, or that it should have something with `.should('stuff')` where stuff could be one of the following:

```js
.should(chainers)
.should(chainers, value)
.should(chainers, method, value)
.should(callbackFn)
```

Chainers means [certain assertions](https://docs.cypress.io/guides/references/assertions#Common-Assertions) to be filled such as `have.length` and `exist` or `not.exist`.

For some cases, [`contains(value)` and `should('contain', value)` might seem similar](https://stackoverflow.com/questions/50042670/cypress-test-is-contains-equivalent-to-shouldcontain) but the former actually returns the DOM element that could be used for further chaining and the latter is used strictly for assertion, the first returns a generic failure as it was not found, while the latter will be an assertion failure.

This `should` could still be used to further chain other interactions, after it passes the assertion.

## Notes

Here are some notes gathered per use case, most API's have certain rules to happen, and other behaviors related to assertions and timeouts chained to it.

### Clicking Checks, Inputs and Divs

`check([arrayOfValues])` could be used instead of writing similar statements to check certain boxes, where the array of values represent the values of which you want checked. 

Most forms and inputs have trivial handling where you would `get` then `check` or `select` to have certain values chosen, but other programmatic type inputs could get tricky, where its actually a div handled by JS.

If the div has a click handler, it would be as trivial as clicking that div from the list of divs, inside a container with `cy.get(container).contains(value).click()`.

### Typing into Fields

A `type(value)` is used on input text fields to trigger some kind of reaction such as to wait and return search results, which can be further used by pressing enter with `type('{keyboardKey}')` that basically [tries to press the keyboard key](https://docs.cypress.io/api/commands/type#Arguments) instead of typing its value.

### Alerts and Other Events

To capture certain events that are related to window and not the DOM, its useful to explore `window: alert` and [other methods](https://docs.cypress.io/api/events/catalog-of-events).

For alerts though Cypress will auto accept them, and that can't be changed, instead we could capture these values with `cy.on('window: alert', callback)` where we could do further assertions on the callback.

### Oh The Places You'll GO.

With `cy.go(value, ?option)` you could go forward and backward with either `forward`, `back` or `1` and `-1`. 

### Map Iteration

Instead of `.map()` though, Cypress uses [`.each(callback)`](https://docs.cypress.io/api/commands/each#Timeouts) with the callback having the following arguments: **value** of the current iteration, its **index**, and the selected **collection**.

### Fixtures, and Captain Hook from Mocha Returns

What seems to be similar hooks, is exactly that, Cypress burrows the concept of `before(callback)`, `after(callback)`, `beforeEach(callback)` and `afterEach(callback)` hooks to inject dependencies and do cleanup on each test suit and test case.

Hooks are super useful in one case of Cypress testing, which is adding fixtures to the testing environemnt with `before` and `beforeEach`. The access is granted with `cy.fixtures(fileName.json).then(callback)` with callback having the data object. Saving it to `this.variable` is the way to access it again later in the code.

### Your Own `cy.command`

In a normal setting, testing suits could go a long way being written as it is, but sometimes, suits could be its own things. The following code for an example:

```js
	describe('name', function() {
		it('test', function() 
		{
			cy.visitThis();
			cy.doThis();
			cy.doThat();
		})
	})
```

Now you could build out a command so that it directly represents the process above in `/supports` folder with:

```js
Cypress.Commands.add("doAllThis", () => {
			cy.visitThis();
			cy.doThis();
			cy.doThat();
})
```

Then the test becomes

```js
	describe('name', function() {
		it('test', function() 
		{
			cy.doAllThis();
		})
	})
```

Its another abstraction that allows you to move the lines and keep the tests clean.

## Design Patterns - The Page Object Model (POM)

One quick thought that comes in mind when reading the above tests, is that it could quickly get out of hand, between the pages and elements that you have to consider, the test cases that you must write for, and the validations and logic that goes behind each test suite. 

The idea behind the POM is to remove one of the above concerns, since we already discovered how we could move repeatable logic to custom commands, is we would also need to move the DOM element calls to its own thing, that is the class.

Its done with basic class syntax, followed by methods that describes and executes how the DOM element is reached with `gets` and the rest of the commands, and then follow that up with the purpose of that method such as entering passwords or choosing options.

```js
import pageClass from './page';

	describe('name', function() {

		const page = new pageClass();

		it('test', function() 
		{
			page.visit();
			page.doAllThis();
			cy.validateThat();
		})
	})
```

Here we would be sure that the methods are in correct context, and the logic is contained within the class.
