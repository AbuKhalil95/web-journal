# What was understood as the Need For Redux

Most important takeaway is that side effects ( HTTP requests) should be done inside components or action creators only. Synchronous data manipulation should be in redux reducers with no mutation of original state.

A thunk is the way to handle these side effects, using redux toolkit to send a dispatch request not with actions, but with normal functions. The functions could then be async awaited, until the operation is done to finally dispatch the redux action.

Important
A revision of thunk syntax is neededز
