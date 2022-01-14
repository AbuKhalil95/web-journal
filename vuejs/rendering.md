# Conditional Renders and List Vue's

Directives are really awkward to work with as a React Developer, since you might have gotten too used working with declarative programming with pure javascript. Though the transition might be quick since directives are of the same declarative type.

## Conditional

For a single element `v-if` followed by `v-if-else` or `v-else` is enough to do the job. In case of block elements that you would want to conditionally render, its best to use a `<template/>` element that wraps what you want with the directive within it, such as: 
```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

`v-show` is another option that toggles the display on the block, so its still with the DOM. Its its own thing so don't try to use it with `template` tag or `if-else` directives. Its a much simple toggle, but `v-if` is lazy so that nothing happens if the value was falsy on mount.

## Looping

When displaying multiple elements that is from a list, `v-for` is used to do the job. The block has full access to parent properties, allowing for props to be used within too as the following:

```html
<ul id="array-with-index">
  <li v-for="(item, index) in items">
    {{ property }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

Its also possible to use ***objects*** and iterate through their **value**, **keys** and **index** as the **first**, **second** and **third** argument respectively.
 
```html
<li v-for="(value, name, index) in myObject">
  {{ index }}. {{ name }}: {{ value }}
</li>
```

### Mutations

Array mutations are handled in a way that triggers the component update by Vue.js, this helps keeping the render up to date. The methods are: `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `sort()`, `reverse()`. But some changes does not trigger it, such as `filter()`, `concat()` and `slice()`, as these return a new array that needs to be replacing the original to cause it with: `items = items.filter(item => item.message.match(/Foo/))`. The process is effecient and changes are compared and inserted instead of replacing the whole array.

### Filtration

What is needed, if the original is kept and new array is used, is to move the computation to the `computed` so that its made on mount and rendered appropriately. But if its necessary to dynamically do so at runtime, a method is applied instead such as:

```html
<ul v-for="array in sets">
  <li v-for="n in function(array)" :key="n">{{ n }}</li>
</ul>
```

```js
data() {
  return {
    sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
  }
},
methods: {
  function(array) {
    return array.filter(item => condition)
  }
}
```
### Notes 

An unexpected turn with an Integer instead of arrays or objects, `v-for="n in 10` causes the template to be rendered 10 times. And just like `template` with `v-if` its also possible to render whole blocks multiple times with `v-for`. 

`v-if` and `v-for` on the same block causes `v-if` to compute first, so it won't have access to scoped information inside a `v-for` block.

Just like with a react list, a `:key="item.id"` is used to properly maintain the order of the list. Otherwise, for simple renders its OK to not append such keys to each list item.

And lastly, components with `v-for` don't get the passed data automatically, instead it needs to be passed in manually with `v-bind` as such:

```html
<my-component
  v-for="(item, index) in items"
  :item="item"
  :index="index"
  :key="item.id"
></my-component>
```