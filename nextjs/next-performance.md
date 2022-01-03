# Next Websites built to be pre-optimzed

One of the elements that makes next.js attractive is one aspect that handles web optimizations, even if partly, really well.

These optimizations include `getStaticProps()` and `getServerSideProps` as they greatly reduce the bundle needed to render the page on the client side with `getStaticProps` by simply providing static or server rendered files, and the ability to time caching differently, [per page as needed](https://nextjs.org/docs/going-to-production).

Static rendering improves time-to-first-byte (TTFB) by rendering content at build time rather than per-request.

## But First, Terminology.

Rendering on Nextjs could be quite confusing, [the following information](https://developers.google.com/web/updates/2019/02/rendering-on-the-web) has the most important aspects that are widely used for rendering on the web.

```md
- SSR: Server-Side Rendering - rendering a client-side or universal app to HTML on the server.
- CSR: Client-Side Rendering - rendering an app in a browser, generally using the DOM.
- Rehydration: “booting up” JavaScript views on the client such that they reuse the server-rendered HTML’s DOM tree and data.
- Prerendering: running a client-side application at build time to capture its initial state as static HTML.

And the following keywords will be used to represent certain performance metrics that are relavant for each part of code performance optimization.

- TTFB: Time to First Byte - seen as the time between clicking a link and the first bit of content coming in.
- FP: First Paint - the first time any pixel gets becomes visible to the user.
- FCP: First Contentful Paint - the time when requested content (article body, etc) becomes visible.
- TTI: Time To Interactive - the time at which a page becomes interactive (events wired up, etc).
```

## What Happens on the Server?

First thing that happens is that the full HTML gets compiled and rendered, therefore all content related work is served on first data fetch, this is represented with `FCP` and helps avoid long `TTI` by reducing large JS bundles after executing DOM related JS first. 

The catch is that heavier pages and bigger first party JS causes longer `TTFB` as no data can be sent until that page has its HTML fully rendered on the server.

### Get the HTML Statically.

Static generation makes sure of *green* speeds for `FCP` and `TTI`, this allows for server rendering that exactly means that no JS (CPU-bound process) will run once it reaches the user

A balancing act is required to see which pages will require user heavy JS, but relatively low amount of statis JS is ideal for SSR generation and likewise for NO SSR option the other way around.

Heavy loads that is expected with user interactions could be made with `prefetch`, as it will load, when possible, the other resources that is expected to load anyway such as:

```html
<link rel="prefetch" href="/pages/next-page.html">
<link rel="prefetch" href="/js/chat-widget.js">
```

This is done by default on Nextjs, with any `<Link>` that has default `prefetch={true}` visible in the viewport to automatically load up the resources needed to quickly render the next possible page load. **Do note that only static rendering has its first party JS preloaded in the prefetching.**

#### Note: Static vs prerendering

Static rendering means not much client side JS will be used to make the page interactive, while prerendering helps out on the aforementioned rendering metrics `FCP` for resources that will have to be processed on the client side anyway to make it interactive.

A quick check can be done by disabling JS on the browsers, and check if the content is still mostly there, if not its then prerendered and is not statically rendered.

### Get the HTML Serverly

Server rendering comes at an obvious cost of computing overhead, since most computation is now moved from the client side to server side, one aspect that gets affected is `TTFB` since some data can duplicate causing unecessary bytes to be bundled and sent.

Techniques that help out reduce SSR overhead include:
- [Component Caching](https://medium.com/@reactcomponentcaching/speedier-server-side-rendering-in-react-16-with-component-caching-e8aa677929b1): The docs are related to react and seems possible on [Nextjs](https://github.com/vercel/next.js/issues/1210).
- Memory Consumption Management
- Memoization Techniques: For every same input the output stays the same, the best next thing to do is to memoize the result as to return that same result directly and skip its computation.

SSR therefore could generally mean less work for client, but more work for servers, and is slower than simply serving statically generated content.

So what SSR provides is the ability to handle live data such as queries and params that usually can't be handled statically (unless the possible params are also known/static such as slugs).

#### Note: Check out service workers

## What happens on the Client?

Rendering, if done fully on the client, gets all its necessary information, JS and other resources on round trips to the server with multiple HTTP requests, and can only match server side rendering if the content is minimal with tight JS budget ([around 100ish KB](https://mobile.twitter.com/HenrikJoreteg/status/1039744716210950144)) and minimal round trip calls,  along with a proper sitemap.xml just to keep the SEO proper.

Luckily Nextjs comes with automated code splitting and possiblility to lazy load on demand or user Interaction such as [loading on scroll](https://www.better.dev/lazy-loading-next-js).

#### Pure SPA is out of focus of this document but worth a [look at](https://developers.google.com/web/updates/2015/11/app-shell).

## Other notes

Code bundling is another aspect that provides performance benefits provided by webpack. As it automatically handles code splitting, compartmentalization so that each page has the necessary JS to run its code without worrying about scope or if it was properly included into another component.

The build logs is another place to look at when trying to check where most performance gained could be done. `First Load JS` that represents all the components and any custom code that comes from `_app` and `layout` components, so any changes to these two components will reflect on all pages.

Changes include removed unnecessary code and dependencies, and making sure if treeshaking works or not by trying to see bloated frameworks and replace it with own code. (Possible solution)

Dynamic imports promotes per need basis of loading component code into the page instead of page load, along with a fallback to show loaders and such, although what **needs** means is different, on one hand it could mean when we would import on event only such as use input with the following:

```js
	<input
		type="text"
		placeholder="Search"
		onChange={async (e) => {
			const { value } = e.currentTarget
			// Dynamically load fuse.js
			const Fuse = (await import('fuse.js')).default
			const fuse = new Fuse(names)

			setResults(fuse.search(value))
		}}
	/>
```

Or it could mean when you would either want to dynamically load it into the DOM on the SSR or on the client side with SSR false, this is a feature of ***component based code splitting***. 

Another code splitting that is handled by NextJS routing is ***routing based***, where these small chunks of code are then easily transfered and handled per page/route.
