# Here at the base app, is the dangers of disabling automatic static optimization

What defines static websites is the pre-rendered pages that are built on build time, and does not change on each request, one thing about having a custom `_app` is that you would want some kind of special `getInitialProps` in order to wrap whatever response with the needed functionalities (redux store for an example).

Once that happens, what is needed is to make sure to define each page at the `pages/**` directory with its own `getServerSideProps` or `getStaticProps`.

The main uses that was configured with the custom component are the following:

- General Layout design, along with possible theme configurations that could be needed, more at [Layout Config](#layout-config).
- Passing states and props between pages with `{pageProps}` that could be then passed down to `<component>` that changes according to the page.
- general data fetching pre loadout for any global configuration such as global props, contexts and store configurations for redux etc.. [more is explored at here](../redux/toolkit/setup.md)
- Global CSS, but should be avoided if page is not statically rendered, since each component could contain its own styles with `styled-component` or others for ease of modification.

## Layout config

```js
function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```
Its possible to only apply certain layouts to certain pages with a custom method that returns the output with:

```js
// pages/index.js
import Layout from '../components/layout'
import NestedLayout from '../components/nested-layout'

export default function Page() {
  return {
    /** Your content */
  }
}

Page.getLayout = function getLayout(page) {
  return (
    <Layout>
      <NestedLayout>{page}</NestedLayout>
    </Layout>
  )
}
// pages/_app.js
export default function MyApp({ Component, pageProps }) {
  // Use the layout defined at the page level, if available
  const getLayout = Component.getLayout || ((page) => page)

  return getLayout(<Component {...pageProps} />)
}
```
