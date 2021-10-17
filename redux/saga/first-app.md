# Debugging the first app

One of the unexpected complexity with Next.js using Redux Toolkit and Redux Saga and was tackling how they all work together, as the first impression was quite simplistic and each tutorial was about each alone, or any two of these.

Debugging the setup of all three together instead of taking the time to implement each in a simple app was a fun experience of deep frustration (relating `/_app.js` configurations of Next.js with toolkit store config and saga middleware implementation) and eventual success (forgot to call the generater functions at root saga after a full day of headache of no logs showing).

## What is Saga.. for real?

I have gotten to know [two ways of dealing with async functions](https://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559#35415559), one using the **traditional javascript** with async await, timeouts and promises. Along with action extraction to reduce duplication and the [easy store singleton](https://react-redux.js.org/api/hooks#usedispatch) that is not recommended, because server side react and singleton store don't work out of the box!

And another using **redux-thunk** to help with the unecessary passing of dispatched to every other component as props, and have both synchronous and async dispatch to look the same intsead of being two different things! (action creators vs callbacks), plus the added benefit of [seperating smart and dumb components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0).

Saga is somewhat different, as it caters to more of bigger apps with tons of actions, instead of overloading the thunk with service files, it might be useful to activate each step of the process as an action itself. This is where Saga becomes useful as it contains *middle-man* listeners that catches actions specific to Saga, then reroute then incrementally to other actions if needed, then finally to the reducer itself while also having access to that reducer state on each step.

Assume an example of a single user process that might take tons of effects or dispatches to accomplish, such as loggin in, and taking previous cart if exists, and previous page that the user was in before logging out, and any other server side requests that might be useful. All of these requests could be done sequentially and in a manner that is transactional and predictable with Saga from the package with no uncessary extra considerations. In thunk you would need to write a queue storage in order to behave similarly, where each action should be preceeded by and continued on specific actions along a pre-defined path. *[Apparently this saga term is very common in the transactional data realm](https://microservices.io/patterns/data/saga.html).*

Best part for big applications? That flow is [TESTABLE](https://stackoverflow.com/questions/34930735/pros-cons-of-using-redux-saga-with-es6-generators-vs-redux-thunk-with-es2017-asy).

## Saga Usage Tutorials

- [Saga Documentaion](https://redux-saga.js.org/);
- [Toolkit and Saga](https://www.youtube.com/watch?v=2hZhIoRuua4).
- [Next.js and Saga](https://github.com/bmealhouse/next-redux-saga). *useful to know how  it worked and know why it is deprecated*.
- [next-redux-wrapper And Saga](https://github.com/kirill-konshin/next-redux-wrapper#usage-with-redux-saga).

Each of these links taught me why I had some trouble getting the initial setup of all three together. Beginning with Calling the generator functions

## The Basics

Getting to know the **generators**, they are not a `function`, but rather `function*` so no, you cannot use arrow function as generators since they are totally different things despite the similarity.

Generators are usually declared first as *watchers* as in each ***main*** generator gets defined first and acts as asynchronous listeners to specific actions, this allows for multiple main generators acting as paralell listeners at once when combined with what is called a **rootSaga**.

This is the main generator which redux will run at store initialization  and `yield all([arrayOfMainGenerators()])`. The generators are called inside the array to initialize the listening process as a work thread.

Generators contain yield to indicate a specific action to be dispatched, but the how is specified after it, such as `all()` that is identical in behavior to `promise.all()`. Another famous API is both `take(pattern)` and `call(fn, args)`, which *take* will tell the generator to pause until the pattern is dispatched as an action and continue to the next steps, and *call* that moves the process to another function or generator or promise.

`put` is a way to allow the middleware to dispatch its own pattern for other generator functions to catch.

Most considerations of what to use where depends heavily on the flow, and if asynchronous is needed at the middle of these multiple dispatches and how to handle them totally depends on the logic pattern the code is based on.

Sometimes a seperate parralel middleware is needed that we don't care if it is done before or after its parent, called `Fork()`, this helps move the logic away if its unrelated but still handle it locally, if the process should not be handled anyway `Spawn()` is used instead and acts independantly.

`race()` is an interesting part that literally takes the winner of two asynchronous processes, such as when you would want to fetch a big chunk of data, but allow the user to cancel at the middle so that the process would also cancel on demand.

## More with experience

These seems suffecient to start digging at existing codebase and implement a design that matches the needs for any Saga redux for now. Additional features and requirements is well explained in the docs.
