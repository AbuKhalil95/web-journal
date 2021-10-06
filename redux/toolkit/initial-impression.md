# Taking a quick detour to Udemy short course

Most of what was seen was quality of life changes.

createSlice({Reducer Object}) has state changes that looks like its mutating, but not thanks to immer. 100% Shorter code without even needing to return the rest of the state properties.

Connect it with createStore(slice.reducer) for one slice, multiple slices would need combineReducer  from redux or configureStore from redux-toolkit that takes in object for configuration.

```js
{
  reducer: {myFirstSlice: Slice1, mySecondSlice: Slice2 }
}
```

No more action identifier worries. Just Export const myAction = Slice1.actions and import it in the appropriate component, then dispatch(myAction.method(payload))
