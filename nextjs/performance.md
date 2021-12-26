# Build to be fast

One of the concerns when doing static or server side generation, _other than SEO_, is how fast can your page load. Many metrics are used to indicate such speed but can never measure raw speed. One of the tools used in this example will be [lighthouse](https://developers.google.com/web/tools/lighthouse).

## Lighthouse as a speed test

This tool provides many useful insights as metrics, of these metrics we will be speaking about, performance will be the topic of this page.

**Performance** seems to be defined by 6 different measures, these are:

- First Contentful Paint
- Speed Index
- Largest Contentful Paint
- Time to Interactive
- Total Blocking Time
- Cumulative Layout Shift

### First Contentful Paint (FCP)

This represents the moment the page loads, to the moment the first content renders, 1.8s represents a good score. Its usually improved by reducing the amount of resources being sent from the server to client, such as:

- Render blocking resources such as JS and inline CSS, along with `<script>` and `<link>` that does not have `defer/async` attribute and is not `disbaled` or has `media="all"`. **This part is handled by next well with code splitting.**
- Minify css in a way that reduces the lines of style with combinations and perhaps whitepaces using a minifier that employs smart byte saving tricks to help in that area.
- Preconnecting or preloading to another domain with `<link rel="preconnect/preload">` which allows the browser to establish a connection asap, while the other orders a full download asap. This consumes some CPU early on, but helps immensly in terms of snappiness and responsive load for critical resources that needs to be downloaded first anyway.
- _Time to first byte_ could do more reading on this, but seems to be network specific.
- minimize redirects, as each would restart the process of loading resources.
- Avoid big network payloads, by keeping the total under 1600 KB, which takes 3G networks some time to fetch but not too long (_around 10ish seconds_) by doing the following:
  - Defer using [PRPL Pattern](https://web.dev/apply-instant-loading-with-prpl/)
  - Minify and compress data (looks like a default nextjs behavior)
  - Use [WebP images](https://github.com/imagemin/imagemin-webp) as the superior choice in websites, or compress JPEG to the level of 85.
- Cache the request.

### Speed Index

### Largest Contentful (LCP)

This represents the percieved page load that users have to wait in order to start seeing the main content, which usually is around 2.5s for the best experience. The following represents LCP:

`<img>`, `<image>` inside `<svg>`, `<video>`, elements with `url()` css, and html blocks with text children. And the value is determined by the load of the largest element of these, in terms of width and height and not byte size, visible to the user viewport.

Recalculation of size or position does not cause a new LCP trigger, only the original render does, which affects elements that are rendered off-screen then enters viewport (not counted), or those that are rendered on viewport then goes offscreen (counted). The calculations are finished once the page are fully loaded, or is interrupted by user scroll, tap or keyboard click.

Resources are calculated by their load time instead of render time for security reasons, to enable for render time we would need to enable [Timing Allow Origin](https://developer.mozilla.org/docs/Web/HTTP/Headers/Timing-Allow-Origin).

A good measure for LCP to handle it directly is to use [`getLCP` from `web-vitals`](https://github.com/GoogleChrome/web-vitals/blob/master/src/getLCP.ts):

```js
import {getLCP} from 'web-vitals';

// Measure and log LCP as soon as it's available.
getLCP(console.log);
```

With some similarities to FCP, [LCP optimizations are available to follow](https://web.dev/optimize-lcp/)
### Time to Interactive (TTI)

According to the MDN docs, TTI reflect the time it took for the last **long task** to finish followed by 5 seconds of inactivity, this is important since the **long task** represents a task that blocks the thread for more than 50ms, they could be:

- Long Running Event Handlers
- Expensive Reflows (and re-renders for react)
- Browser work with the event loop that takes more than 50ms to finish.

![TTI image](../resources/tti.svg)

Most of the performance fixes are applicable to others, in addition to two points that were not mentioned:
- Minimize chaining critical request, which can be ebst explained as resources critical to render, that are grouped together causeing long blocking time, important to notice and can be fixed by deferring, ordering load times with preloading or optimizing its sizes.
- Keeping resource counte low and transfer sizes small, which is already done with `next.js` via code splitting and tree shaking but could use more [scrutiny to make sure its working well](https://stackoverflow.com/questions/64485105/all-nextjs-pages-have-almost-the-same-js-bundle-size). 

### Total Blocking Time

### Cumulative Layout Shift
