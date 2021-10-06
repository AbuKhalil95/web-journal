# Documentation - Udemy quick walkthrough

In addition to SEO, this nextjs framework blends server and client side code. Other features are simplified react solutions such as file based routing. Since its still server side work, ability to add server code and add support for database connection.

How next handles dynamic routing is first add a dynamic file holder with [file-name].js at the intended subfolder, then make use of a custom react hook by next.js that is useRouter() , of which router.query.id param could be extracted and used.

Just like react router, you could also push into the router hook with router.push(stringPath) .

For SPA apps, Link components are needed and are imported from next/link instead of the usual react router component.

_app.js is a special next.js component, that shows the page component on the file structure, and pass in any page props to it. It's possible to wrap this part with a general app wide layout to keep headers for an example consistent.

Separation of components and pages is the way to do it in next, already had some experience with this file structure with pure react as it was "leaner".

Pre-rendering is the holy grail of SEO optimization, its best to have content ready to serve to google bots to work out what kind of website this is, than perfectly load every component half a second later causing it to miss most of these robots.

One of the function for pre-rendering is called getStaticProps() as it sounds as, will get the props before components are called. revalidate initializes the server to refresh the props every x seconds.

getServerSideProps is quite different, it deals instead with each request and populates its own fresh props with each recieved on time.
