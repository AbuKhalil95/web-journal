# Starting with the [ABC's](http://learnyouahaskell.com/introduction)

With the link, its safe to assume we will be coasting the Haskell knowledge - from the POV of newbies such as us - until it clicks, so lets get started.

## What Matters in Functional Programming

Apparently, I have been running functionals for a while, in fact all of us did at the JS world, albeit not as purely and with shortcuts added.

### Purity

When you run a program, its usually you telling the program how to change. Here instead, its what that change is exactly, thus any function here is a very, pure function.

Once you have that function at the ready, composing additional functions that looks simple at first, could become a functional behemoth that does exactly what you need, in a very predictable manner.

### Laziness 

Evaluation of code happens, only once you need it to happen. That means nothing is held into memory unless it was explicitely asked to. Laziness allow for otherwise impossible operations due to size limit, such as working with infinits? 

Working this way allows for a neat, sequential data manipulation that in addition to its inherit predictablity, is also effecient.

### Static

Having both statically typed system and type inference seems like a leap for that kind of systems, thus allowing you to catch most errors at compile time, leaving very little room for runtime errors. 

### And Is Smart

Or atleast built by smart people, haskell means a lot more when it comes to being concice and elegant. And to understand the beauty that is in the eye of the beholder, we have to try and become one ourselves.

## [Lets Dive In, Then!](first-steps.md)