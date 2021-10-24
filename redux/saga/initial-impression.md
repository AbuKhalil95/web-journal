# Saga with [some youtube](https://www.youtube.com/watch?v=Om4Lb8c5Lbg&list=PLMV09mSPNaQlWvqEwF6TfHM-CVM6lXv39&index=5)

## First things first
What was quickly taken in is the concept of generators at Saga, these generators act as an await along with reducer that has strict ordering for what goes after, just like an unskipable staircase of multiple steps.
The syntax is as following:

```js
function* dataGenerator(param) {
  const returnData1 = yield 'data1Action'
  return returnData1;
}
```

where `*` is the notation needed when defining a generator, this acts a function that returns an iterable that could be called with `dataGenerator.next()`.
This will have a return obj of `{value: data1Action, done: false}`, as you could notice by now that done signifies when should the Saga knows if everything has returned yet, since it has not we would need another `dataGenerator.next()` to really reach the return obj of `{value: undefined, done true}`.

The value of the last iteration was not `returnData1` because nothing was passed into the `next(arg)` argument.
This is apparently very useful if we want to pass in certain values first before moving on to the next step as a part of a big asynchronous function.

## How Saga Works Externally
Saga at first operates as a **Middleware**, where the store config would look like:

```js
const storeConfig = () => {
  const sagaMiddleware = createSagaMiddleware(); // imported from redux-saga
  const store = createStore(AllReducers, 
    compose(
      applyMiddlware(sagaMiddleware),
      //...any other middlewares, extensions
    )
  )
  sagaMiddleware.run(Saga); // this is the Saga file logic
  return store;
}
```

That `.run(Saga)` part is the file we will be filling, with `yield takeEvery('actionType', callback)` we could intercept all dispatches with `actionType` type and do the callback afterwards.

Best part is that `takeEvery` is only one of the useful many other keywords provided by Saga, another is `takeLast` that basically handles your multiple clicks on that submit button and only takes in the last click before initiating the callback.

The callback could then be further used to dispatch actions back to the reducer or Saga middleware again with another `yield put({ type: 'AnotherAction' })`.

## Yield API's ( put, takeevery, take, call, select, fork )
- `put` is an API that dispatches an action, or to another channel that takes that action instead.

- `take` is very similar to `takeEvery` except that it only takes in the `actionType` or `callback`.,and instructs the middleware to move into the next yield and only once while the middleware is still being evaluated? need more info/examples.

- `call(callback)` after a `take` basically fires the callback right after it detects the `take` action dispatch.

- `yield fork` is a unique operation similar to `call` but is specifically non-blocking as in it could handle multiple forks running simultaneously on the fly.

The order as its defined at the `Saga` is important since it is simply about what actions would move the iteration to the next line of code. So any action that does not comply with the order is simply ignored.

One other useful API is `yield select(selector)` where selector is very similary to the normal redux useSelector that invokes the state to return something needed.

There are more interesting concepts that look convoluted at first such as `throttle`, but helps immensely when trying to do custom effects on too many react components. What it does is delaying the listener of an action with `throttle(delayInms, action, callback)`. It is basically similar to hooks and timeout but is easy to configure multiple layers of it and connects to redux store (listens to actions rather than components).

