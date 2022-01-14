# The Bureaucracy of Events and Forms

- The basic usage calls for the directive `v-on:event="callback"` or `@event="callback"`. 
- An inline statement to bind a certain input to the event is trivially done with `<button @click="say('what', $event)">Say what</button>`.
- `$event` is a special variable that takes the event and passes it through the method as a second argument.
- Passing multiple event handlers is possible with a simple comma, `@click="one($event), two($event)"`.

## Event Modifiers

Such cases of preventing default is the most common modifier used on form submit, others are provided by Vue.js as such:

- `.stop`: Event propagation stops at that element.
  
- `.prevent`: Prevent default modifier.
-  
- `.capture`: Captures any event meant for children first, then moves on to that child.
 
- `.self`: Events have no effect if the target was not the element itself.
- 
- `.once`: Tries to trigger the event once only.
- 
- `.passive`: Ignores any `event.preventDefault()` calls at the handler.

Event modifers can be chained to provide a more convenient syntax for multiple modifiers: `<a @click.stop.prevent="doThat"></a>`, but be careful of the order as it matters, where `@click.prevent.self` prevents both self and child default behaviors, but `@click.self.prevent` will only prevents self element default behavior.

## Keyboard Events

By converting keyboard events as [mentioned in the MDN](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) to *kebab-case*, its possible to listen to these events within listeners as `<input @keyup.page-down="onPageDown" />`. 

Keys could be chained so that simultaneous key presses is captured with `<div @click.ctrl="doSomething">Do something</div>` of both `ctrl` + `click`. 

If only certain combinations are needed, an end chain of `.exact` is needed to capture the specific key presses.

## Form Inputs

`v-model` a magical Vue.js solution to handle updates, but input attribute of `value`, `checked` and `selected` are ignored and initial value should be reflected at the component data prop instead. The variable passed into `v-model="variable"` corresponds to the data key property.

To handle all input types, `v-model` has the corresponding properties and events:

- **text** and **textarea** elements use `value` property and `input` event.
- **checkboxes** and **radiobuttons** use `checked` property and `change` event.
- **select** fields use `value` as a prop and `change` as an event.

As you could imagine, its possible to return a custom value, out of `checked` as its normally boolean, with `false-value="variable"` and `true-value="variable"` for checkboxes, and `v-bind:value="a"` for radio buttons. Object literal is another possible return with `:value="{ number: 123 }"`.

## Form Modifiers

- `.lazy`: Accepts new updates on `change` that is similar to the event listener, instead of on `input`.
- `.number`: Coerces string values to a number, usually done with input type of `number` but this is an alternative with text input.
- `.trim`: Removes whitespaces before and after user input.
