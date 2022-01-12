# CSS Syntax Galore

With Vue.js, you might have too many options to append classes and ids, but they come for specific conveniences.

## The Object Way

This syntax requires the css class to be the key, in the key value pair that is passed to the `:class` shorthand binding. The class is only toggled into the element if its value prop is of truthy.

```html
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>
```

Or it could be grouped into a variable prop as a better syntax of having multiple classes with `<div :class="propObj"></div>`

## The Array Way

We could also simply pass an array of variable props, defined at `data { varClass1: class1, varClass2: class2 }` such as `<div :class="[varClass1, varClass2]"></div>` or even including `[isTrue ? varClass1 : '', varClass2]` to conditionally render these list of classes, and even alternatively with `{ varClass1: isTrue }` that combines with the object syntax to apply the conditional render.

## The Component Way 

Classes passed into components as props with any of the above methods gets appended to the existing classes in that component instead of overriding it, the tricky part is if you had no single element that wraps a component ( allowed in Vue.js ), so that the recieving root element needs to be defined with `<p :class="$attrs.class">Hi!</p>`

## Inline Styles

In a very similar fashion, Object syntax `<div :style="styleObject"></div>` and Array syntax `<div :style="[baseStyles, overridingStyles]"></div>`, and to gather a bunch of [prefix values](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) `<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>` so that the support covers multiple browsers.
