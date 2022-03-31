# The terminal with the [Glascow Haskell Compiler (GHC)](https://www.haskell.org/ghc/)

## Arithmetics

Once the compiler is installed, many arithmetics similar to what you could do in node, can also be done such as `5 + 5 = 10`, `5^5 = 3125`, `5 * (-3) = -15`, and even boolean operations and equalities. 

But not `5 + "llama"` or even `5 + "4"` and `True == 5` since types don't match at all. Still, some work can be made with `5 + 0.4` as numbers could be changed around.

## Functions

So far, we did no "functions" but yet, we actually did with `*` and `+` which are called ***infix*** functions, that are sandwiched between two nuumbers and gets out the result.

Others are called ***prefix*** functions, which are of syntax `name parameter parameter` with spaces as seperation.

These prefix functions with two arguments can still work as an infix, quite neat right?

Sequential functions are run with parenthesis, and unlike other languages, does not denote arguments at all as `bar (bar 3)` is not `bar` and `3` as arguments and is instead `bar` calling `bar 3` after `bar 3` gets evaluated.

