# The Pre-made Security

Perks of being a java developer, being able to compile new dependencies using [Spring Initializr](https://start.spring.io/), configure your classes to suite the new system `@Bean` and other [dependency injection style of `@annotations`](https://springframework.guru/spring-framework-annotations/), and you would be ready to go.

## Security Building Blocks (Auth)

The first piece of the puzzle involves authentication, which is not same as *authorization*! This is a classic case of who are they and what are they allowed to do, in this case involves seperate logic for each in Spring.

### Whoami

*`AuthenticationManager`* interface is the starting point, it has one method of `authenticate()` that *returns* `Authentication` or null, depending on the input, or throws an `AuthenticationException` if no validation has been reached. The exception is dealt with in a generic way so it could just display failed authentication.

Another layer of or a different implementation if that `AuthenticationManager` is the `ProviderManager` that helps manage multiple instances of `AuthenticationProvider`'s that holds an extra boolean returning method to indicate whether that kind of `Authentication` is supported.

Another parent to `ProviderManager` is given to help deal with fallback `null` values returned form each instance, of which will check if all were null to fail. Otherwise wihout it a single `null` value could prove problematic.

### Managing Whoami

`AuthenticationManagerBuilder` is the way to do so, initialization with the Class instance argument help configure jdbc authentication which is commonly used in web applications along with other chained methods such as `.withUser('user').password("secret").roles("USER");`, looks really similar to another builder in previous chapters***.

`@Autowired` helps bunch the method along with others in a `@Bean` that is as mentioned a global instance accessible from anywhere***. `@override` helps keep it locally instead if needed.

Default spring is a single global `@Bean` `AuthenticationManager` with one user.

### What Can I Do

After authentications, comes authorization, and with great authorization comes greater responsibilty. So with `AccessDecisionManager` that has three different implementations, of all they represent the outcome chains of `AccessDecisionVoter`.

The voter has a vote method that argues along with an `Authentication` and a generic `object` that represents anything the user might want to access, this object would then be [decorated with `ConfigAttributes` interface](https://en.wikipedia.org/wiki/Decorator_pattern) that simply returns a string encoded to provide the intentions of the user such as user role.

`ConfigAttributes` could be changed to handle certain expressions such as `isFullyAuthenticated() && hasRole('user')` which is provided by the `AccessDecisionVoter` to do so, additional customization can be done by extending `SecurityExpressionRoot` and `SecurityExpressionHandler` of which I have no idea how to.. yet.

### Managing What Can I Do

One way to do so is done with **servlet** `Filters` in a chain or one singular `Filter`, a chain of filters requires all be passed in order for the request to reach the servlet, from which each filter can have total veto power to deny the request, or even change it downstream. *(Middleware chaining with next() anyone?)*

`FilterRegistrationBean` is commonly used to order `Filters` in a non-conflicting order, otherwise another way is to specificaly mention `DEFAULT_ORDER of Integer.MIN_VALUE + 50` that tells us that its an early filter in the request chain, but still won't be intruding on even earlier ones.

`FilterChainProxy` is our very own `@Bean` with Spring Security, it has its own chain that shows as a single `Filter` but implements its own `Filters` that have exclusive veto powers on everyone else, kinda like the UN but more democratic.

Dispatching chains of `Filters` could be up to route specific, each route having its own set of chain.

A vanilla chain has the following length of 6 `Filters`:

- 5 chain `Filters` helps ignore certain file paths related to JS and CSS along with 1 `Filter` for our `/error` route.
- Last chain catches all paths and would contain our logic for "authentication, authorization, exception handling, session handling, header writing, and so on.".

Of which could be reconfigured with `security.ignored` from the `SecurityProperties` `@Bean`.


