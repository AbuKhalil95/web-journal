# HTML with pseudo Mustache

The syntax is exactly the one used with [Mustache](https://mustache.github.io/), a double curly brackets `{{}}` that helps out inputting dynamic content to HTML template strings. But with some caveats, or how Vue.js generally deals with different scenarios.

## What Looks Trivial

```html
<span>Message: {{ msg }}</span>

<span v-once>This will never change: {{ msg }}</span>  <!-- v-once directive to avoid updating once displayed -->

<p>Using v-html directive: <span v-html="rawHtml"></span></p> <!-- v-html directive to render the variable as HTML, check XSS vulnerability -->

<p v-if="seen">Now you see me</p> <!-- v-if conditionally renders the element it exists on -->
```

## What might be confusing

```html
<div v-bind:id="dynamicId"></div> <!-- helps render attributes as its not possible with pure mustache -->

<button v-bind:disabled="isButtonDisabled">Button</button> <!-- in case of booleans the attribute will not be included on falsy values -->
```

`v-bind` and other directives such as `v-on` actually takes the attribute arguments such as `id`, `disabled` and `href`, allowing these attributes to be dynamically affected by the value its specified with.

## What is totally new

Rendering JS expressions is possible within Vue.js with the following syntax.

```vue
{{ number + 1 }} 
<!-- but not  -->
{{ var a = 1 }} 
<!-- as its a statement not an expression, user defined globals are no go.  -->

{{ ok ? 'YES' : 'NO' }}
<!-- but not  -->
{{ if (ok) { return message } }} 

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

But its possible to use [other globals](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9) to use within the Vue.js evaluation of expressions.

Another new feature if dynamic directives are needed, or dynamic event handlers as arguments to directives, its possible to use the `[variable]`, where `variable` is expected to be either a string that is going to be parsed, or a null so that the directive is removed, such as:

```html
<a v-bind:[attributeName]="url"> ... </a>

<a v-on:[eventName]="doSomething"> ... </a>
```

For even more complex naming, such as dynamically added attribute naming with string literals or `['foo' + bar]` its best to compute it beforehand as its invalid, also attribute namings are case-insensitives and are defaulted to lower case, best is to keep it as so.

Modifiers to directly mention certain behaviors such as `<form v-on:submit.prevent="onSubmit"> ... </form>` is basically an inline `event.preventDefault()` call. Life made easier with Vue.js.

### Shorthands

`v-bind` could be instead replaced with `:` as `<a :[key]="url">` and `v-on` with `@` as `<a @[event]="doSomething"> ... </a>`.
