# What was understood as the Need For Redux

Redux as a statemanagement is a multi-fauceted tool that does the same thing, provide a way to have a global `store.js` that you could use anywhere in the project, directly. 

Most of my experience revolved around building the store for *React.js* and *Next.js*, each had their own headaches for data fetching that required additional libraries to handle `async` code easily although you could implement your own to avoid the unnecessary complexity.

Most important React.js takeaway about **Redux Flow** is that side effects ( HTTP requests) should be done inside components or action creators only. Synchronous data manipulation should be in redux reducers with no mutation of original state.

`Thunk` is a way to handle these side effects, using redux toolkit to send a dispatch request not with actions, but with normal functions. The functions could then be async awaited, until the operation is done to finally dispatch the redux action, the reason to use thunk is best explained with [the following.](../saga/first-app.md#What-is-Saga..-for-real?)

The rest is [moved to redux-saga](../saga/initial-impression.md) since that is where I will be picking up more about redux.