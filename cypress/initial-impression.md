# Crash Courses and misc videos

The looks and semantics are very similar to other testing frameworks such as jest, but the main idea behind cypress is to be the browser itself. This (being part of the browser and not a hidden dom) allows for otherwise not easily possible end to end testing, and integration testing as if you are running the tests manually.

## [What it is supposed](https://www.cypress.io/features) to give us

- Time travelling: the legal kind, this is possible through screenshots provided to see the visual interactions on each step, and even a video.
- IRL interactions: reloads and waiting happens on real time as if the users are testing it, automatically.
- Extensive control: Need network throttled? Or additional timers or control on server responses? No problems.
- The best part? Debugging just as if you are using Chrome DevTools.

## Ecosystem

Inside, we would find the following: 

- Test Runner that has all the running tests.
- Dashboard Services that will be recording the interactions and results of these tests.

## File structure

- fixtures: as the name suggests, it stores dummy data and "fixed" stuff in order to be used by the suite.
- integration: where the folders and its subsequest test files are grouped into suites of independent test.
- plugins: extends the functionality of cypress, with event listeners and such.
- screenshots: is where failed test cases are screenshotted.
- support: holds any scripts that are just ugly, too long or we want to reuse it.
- videos: is created once video capture flag is turned on (true).

## A peek inside

Each main testng is described with a `describe` along with an `it` that describes each suite that is usually independant. Testing operations api starts with `cy` and is followed by the method we want such as `cy.visit(url-string)` that sends the test to the intended URL.

Another is `cy.contains(text)`, it helps find text in the current screen with `cy`.

`cy.get('selector')` takes the selector and matches it with the current DOM, returning the elements in a way very similar to `querySelectorAll(selector)`. But to access inner content is to use `.should(condition, value)`, where the condition looks like `.should('have.text', 'This value')`.

As you have seen so far, no `let`, `var` or `const` are used to do any operations, and instead operations are dot chained to do the next related opration.

more `cy` operations are `cy.viewport()` helps make sure responsiveness are taken into consideration. (Will need to check if they actually change deviceType).

## What configs that might be useful?

- `--headless (default), --headed` is a parameter that deals with showing the test runnner along with a browser window that executes it, or not. Its generally faster to not show the windows as its less processing that the computer will have to deal with but you won't be able to see what is happening.
- `--spec` is used to specify which single file to run the tests with.
- `--record` helps out in case you need the test [to be recorded](https://docs.cypress.io/guides/dashboard/projects#Set-up-a-project-to-record) for that run.
- [And many more](https://docs.cypress.io/guides/guides/command-line#cypress-run).