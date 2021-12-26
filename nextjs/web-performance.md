# Websites Built to be Fast

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

This index shows how the visual progression of the website looks like on load, it uses [Speedline](https://github.com/paulirish/speedline) that takes literal frames from a video recorder to calculate the duration between the first visual change and last visual change. Histogram is used to display the amount of visual change is made with time.

One main change that could affect this score other than what was mentione above:

- Ensure text remains visible during webfont load. This usually happens due to browsers hiding text until font loads. This can be enforced with `font-display: swap` to the font css or `&display=swap` to the google link, along with preloading web font links.

### Largest Contentful (LCP)

This represents the percieved page load that users have to wait in order to start seeing the main content, which usually is around 2.5s for the best experience. The following represents LCP:

`<img>`, `<image>` inside `<svg>`, `<video>`, elements with `url()` css, and html blocks with text children. And the value is determined by the load of the largest element of these, in terms of width and height and not byte size, visible to the user viewport.

Recalculation of size or position does not cause a new LCP trigger, only the original render does, which affects elements that are rendered off-screen then enters viewport (not counted), or those that are rendered on viewport then goes offscreen (counted). The calculations are finished once the page are fully loaded, or is interrupted by user scroll, tap or keyboard click.

Resources are calculated by their load time instead of render time for security reasons, to enable for render time we would need to enable [Timing Allow Origin](https://developer.mozilla.org/docs/Web/HTTP/Headers/Timing-Allow-Origin).

A good measure for LCP to handle it directly is to use [`getLCP` from `web-vitals`](https://github.com/GoogleChrome/web-vitals/blob/master/src/getLCP.ts):

```js
import { getLCP } from "web-vitals";

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

### Total Blocking Time (TBT)

This represents the time spend on total, for the sum of single tasks that takes longer than 50ms to complete. The visual defines what is counted as 345ms total blocking time instead of 560ms total processing time.

![tbt image](../resources/tbt.svg).

TTI relates to TBT that it shows the severity of the blocking tasks on the time it takes to be fully responsive, such as small 50ms tasks with less than 5 seconds in between acts as a single big blocking task for that duration for TTI, but in terms of TBT, it shows how little impact the small tasks to user experience with a small number, however on large numbers is does reflect worst user experience.

### Cumulative Layout Shift

One of the least performance impact, but most user experience affective score. This is what you would expect if the website gets sudden burst of layout changes while its loading. The layout change, unless required by user input is generally undesireable and could be harmful in cases of made transactions that could be mistaken because a button shifted into the click or to somewhere else.

The score is calculated by using the largest burst (1 second long) of layout shift score that happens during the lifespan of a page, this happens when an element has a position change between frames of 1 second, for a duration of 5 seconds max as the window duration. It's formula is defined with:
`layout shift score = impact fraction * distance fraction` where the impact fraction is the total area of the affected layout shift related to the viewport, multiplied by the distance that occured to the affected elements moving from one layout to the next.

Expected layout shifts don't trigger the score calculation, and includes the following:

- User induced layout shift (any shift within 500ms of user interaction)
- Animations using CSS transform (use scale instead of width and height, and translate to move it instead of top, bottom, left and right).

[prefers-reduced-motion](https://web.dev/prefers-reduced-motion/) is a media query that helps define alternative, slower or less motion for users that has it enabled.
