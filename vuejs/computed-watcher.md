# Vue.js QoL Optimisations

As you could imagine, with longer and more complex statements within `{{ calculation }}`, it could get unwieldy and hard to maintain fast, one quick hack Vue.js provided was a `computed: {}` property within the component class.

## Computed Property

The declarative work shows that its much easier to have computed property that is bound and depends on another that is clear and easy to use. Plus, it caches its result, and only updates on reactive dependencies, so any other functions such as `Date.now()` that is computed, won't get updated more than once.

The alternative is to simply move any simple logic to `methods: {}` that is not necessary to cache or needs to be updated periodically. The syntax is as the following:

```js
new Vue({
...
  computed: {
    var12: function () {
      return this.var1 + ' ' + this.var2;
    }
  }
})
```

This is by default, a getter and can be remade to include a setter that updates the properties accordingly with:

```js
new Vue({
...
  computed: {
  	var12: {
   		get: function () {
      	return this.var1 + ' ' + this.var2;
    	},
  		set: function (val1, val2) {
    		this.var1 = val1; 
				this.var2 = val2;
    	}
  	}
  }
})
```

## Watcher property (generic computed)

A more appropriate computed property that handles custom dependency, useful for super expensive and/or asynchronous operations and want to control what triggers that operation, such as:

```js
new Vue({
	data: {
		return {
			propA: null,
		}
	}
...
  watch: {
    propA: function (arguments) {
      this.prop = arguments;
			this.method();
    }
		// deep: true to enable deep object comparison and not on new instances only.
)}
```

So that the watcher would listen to changes to propA, then run the callback that does its own thing, debouncers are [a cool idea](https://vuejs.org/v2/guide/computed.html#Watchers) to match with API calls that are called frequently.

If the watched value is an object, there are [two approaches](https://stackoverflow.com/questions/42133894/vue-js-how-to-properly-watch-for-nested-data), the first involves watching the nested value by direct access such as `'obj.nestedKeys.watchedKey': callback`, or with `deep: true` that compares key pair values in depth. If none is needed and you just need a single key to watch for, its best to directly use and return that nested key variable on a computed method.
