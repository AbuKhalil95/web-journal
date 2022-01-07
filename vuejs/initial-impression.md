# Vue, as in Your Web View

Vue.js, often seen as the hybrid child of react and angular, seem to take the full potential of both while minimizing the risk of overcomplications. (React Improvement Default) One of these are related to performance changes, since `shouldComponentUpdate` is handled automatically at vue and each component would only update if its dependencies did, instead of re-rendering if its parent got re-rendered too. Another (Angular similarity) is with its pre-defined directives that provide functionalities in a bit of an awkward syntax, for the sake of simplicitiy.

## Similarities

Vue js, in a very similar way to react, uses jsx to store its components, along with its HTML templates for more simpler elements. It supports component based architecture, has similar and separate concepts provided with libraries such as routing and state management.

Other similarities include functionalities such as sharing the use of virtual DOM, composable (building onTop as children and props) and reactive (stateful and customizable) components, plus a big emphasis on core features with the ability to add third party libraries with ease.

## Differences

### HTML & CSS

What react offers, is jsx, everywhere. Even I code css within jsx with `<style jsx>`. Vue on the otherhand embraces classical web expressions with possibility to code with pure html, html templates (use **_PUG_**), splitting css files and importing them per page, etc..

In terms of css, `<style scoped>` provides scoped styling, adds UUID to the end of classes, if required, and is flexible enough to contain normal css, preprocessors and other integrations.

### State Management & Routing

Vuex is part of the core functionalities and therefore has much more robust support and up-to-date integration/documentation. Mic Dropped.

### CLI

Could use more CLI usage first.

## Syntax Quick Dive

Components at vue are simple objects, available globally once it has been declared, and are declared and attached to vue with a new Vue command that accepts el, template and components. `El: #ID` defines the ID of parent element at the base HTML would be, similar to #root but also can be attached elsewhere to express reusability and flexibility.

Vuejs template starts with an object key called `template:"jsx"`. This is where the html string begins for the component. Within vuejs template or jsx, directives such as v-if is used to express logic within the template, it's more closely related to angular in that manner. Another is a more simplified lifecycle methods as it does all the work within the following, created, beforeMount, mounted and beforeDestroy.

States in vanilla vue, similar to class based react component, are defined as data key `data: function() { return value}` and can be manipulated with object methods key that accepts functions defined within it too.

React props, also called props in vue, defines what is accepted within `<Component :prop="value">`, it lives within the props key as an object with key for its name and value that defines the expected type, best defined at the beginning and accessed directly with it's name.

Directives might look awkward since it's Inspired by angular, but might prove to be desired as it's generally more accessible with less requisites need to declare such working.
