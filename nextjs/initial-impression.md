# Documentation - Udemy quick walkthrough

In addition to SEO, this nextjs framework blends server and client side code. Other features are simplified react solutions such as file based routing. Since its still server side work, ability to add server code and add support for database connection.

How next handles dynamic routing is first add a dynamic file holder with [file-name].js at the intended subfolder, then make use of a custom react hook by next.js that is useRouter() , of which router.query.id param could be extracted and used.

Just like react router, you could also push into the router hook with router.push(stringPath).

For SPA apps, Link components are needed and are imported from `next/link` instead of the usual react router component.

`_app.js` is a special next.js component, it stores all requests to pages and passes in appropriate props from the server side. It's possible to wrap this part with a general app wide layout to keep headers for an example consistent.

Having a custom `_app.js` is critical to run custom libraries such as `Redux` and `Redux-Saga`, as they will need to push their own store after each new request ( each single request ) and the only work they would have to do on the server side is to fetch and pass in the state from request to response.

Separation of components and pages is the way to do it in next, already had some experience with this file structure with pure react as it was "leaner", what could be done is that the pages define the structure of components, and holds all props that have to be fetched on the server, components on the other hand could handle its own state and props from redux store.

Pre-rendering is the holy grail of SEO optimization, its best to have content ready to serve to google bots to work out what kind of website this is, than perfectly load every component half a second later causing it to miss most of these robots.

One of the function for pre-rendering is called getStaticProps() as it sounds as, will get the props before components are called. revalidate initializes the server to refresh the props and page every x seconds.

getServerSideProps is quite different, it deals instead with each request and populates its own fresh props with each recieved on time.
