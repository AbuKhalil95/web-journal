# An overview of Web Development with MongoDB and Node.js

## What makes nodejs what it really is?

Apparently, web development had a major drawback with its [blocking i/o](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/) operations before the advent of the node.js backend, with ryan dahl being inspired by the [flikr progress bar](https://www.stoneward.com/blog/2016/06/the-beauty-of-node-js/) that kept accessing backend servers with querries on the progress instead of streaming that info forward.

Another extremely important feature is its single threaded operations, allowing for a more lightweight operation and controlled process environemnt of backend servers that does the heavy lifting of modern web apps. Add thread operations as needed and with precision.

## Asynchronous callbacks

Generally, when doing event driven callbacks, what happens is that the app freezes when an event happen to give room to that callback. Here however, the app is prioritized and the callback is run afterwards, allowing for a more streamlined behavior with multiple events and synchronous operations at the same time.

## And apparently there is more.

### CLI
    - `Grunt.js` which helps automate many everyday tasks.
    - `inquirer.js` that is a simple input waiting command.

### Robotics
    - `Johnny-Five`
    - `Cylon.js`

## The socket.io

One of the best 'interactivity' producing tools that I have worked on so far, allowing for chat and notifcation systems and maybe other  that supplements. 
