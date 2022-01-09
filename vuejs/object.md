# Intro to the [Keys of Vue.js App](https://v3.vuejs.org/api/application-api.html)

Here is how it starts.

```js
const app = Vue.createApp({
  /* options */
});
```

And here is how it ends.

```js
// Root component is what the app mounts first.
Vue.createApp(rootComponent)
  // 
  .component('SearchInput', SearchInputComponent)
  .mount('#root')
  .directive('focus', FocusDirective)
  .use(LocalePlugin)
```

## The [MVVM Pattern](https://en.wikipedia.org/wiki/Model_View_ViewModel)

Vue.js mocks what a viewmodel in the MVVM pattern is like, by using component intances that are generally reuseable in many places. The general file structure is quite similar to React.js, where 

```
Root Component
└─ BigComponent
   ├─ SectionalComponent
   │  ├─ MiniComponent
   │  └─ MiniComponent
   └─ SectionalComponent
      ├─ MiniComponent
      └─ MiniComponent
```

## The Instance, its Properties, and their Multiples. 

Many properties help define what, where and how the component exists within its lifecycle in a Vue.js app, they are split into `user-defined` and `built-in` properties, the first are such as `methods`, `props`, `computed`, `inject` and `setup`. The second are prepended with `$` such as `$attrs` and `$emit`.

In addition to these, are the usual life-cycle properties, such as `created()` that computes whatever after the instance has been created. Others such as `mounted`, `updated`, and `unmounted` are available and are paired with `this` pointing towards the instance it was triggered with.

![vue.js lifecycle](../resources/lifecycle.svg)