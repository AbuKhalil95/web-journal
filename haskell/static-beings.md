# Natural Static

Haskell being a statically typed language, gets its reputation from not only being that, but also with a bit of inference working out most of the annoying need to type things out.

## Why Things Are Easy With Static Typing

Haskell commands the need for *type* and *typeclass*, where type is the member  is known at compile time for each expression and thus some errors are caught at that level.

There is `:it expression` that is used to print the type out, and the print reveals the result with `expression :: Type` that means *has type of*. 

List of characters `[Char]` is a possible result since strings are indeed lists, but for tuples, the length and type of each index is defined by it's own type, such as: `(True, 'a')  :: (Bool, Char)`.

Functions given with types are generally a good idea, and denotes what the input and output looks like, but should be dropped for very short functions.

The following are equivalent.

```haskell
removeNonUppercase :: [Char] -> [Char] 
removeNonUppercase :: String -> String 
```

Also denotes if we have multiple arguments and want to define its type. 
`addThree :: Int -> Int -> Int -> Int` or `Int, Int, Int -> Int` 

To note the resultant type of the function with inference, we could get it with `:t` too.

### General Functions In Static Haskell

**polymorphic functions** are the result of the need to write general functions that has no regard to the type of the input, such as when trying to print out the first value of an array, as in the case of `:t head` that results in `head :: [a] -> a`. Where is a variable of *any* type.

## Typeclass Is Sort Of An Interface Too, That Is A Tad Bit Better.

Typeclasses define how the behavior of a function is expected to be, where `=>` denotes the constraint on which it will operate in terms of input, arguments, and output type so that uniformity in behavior across functions can be made.

Most classes use the basic `Eq` typeclass, where it provides an equality of types in it's argument, such as `:it (elem)` yielding `(Eq a) => a -> [a] -> Bool` that is very similar to `:it (==)` that also yields `(Eq a) => a -> a -> Bool` with a list for the second argument since it checks over that list if the first argument exists in it.

Another useful typeclass is `Ord` with `:t (>)` that defines the comparing functions. With `compare` we could also get `GT`, `LT` or `EQ` comparison results that means greater, less and equal than.

`Ord` members are also part of the `Eq` club. A Hierarchy of membership of sorts. Another membership that can be acquired is `Show` which can turn results to strings. 

`Read` does the opposite of reading the input string to transform by inference to its appropriate type. That inference though is constricted in that we should do some sort of operations to the result, such as subtraction for number inference or appending for list inference.

### Making Sure with Type Annotations

If no operations are provided, we could instead explicitly explain that we want some sort of type instead with ***Type Annotations*** `::`, so that `read “5” :: Int` returns an Integer `5`. Even then the type of number has to be explicit too, as there is no way to infer an integer or a float by the single value alone.

`Enum`, A special class in every programmer's heart. With the following types: `()`, `Bool`, `Char`, `Ordering`, `Int`, `Integer`, `Float` and `Double`. They are known as sequential ordered types.

`Bounded` are basically members with maximum and minimum bounds, you don’t want to be a part of that, but it's good to know one’s limitations within context. The Int of the bound `minBound :: Int` is it's lowest 32 bits value, it's character returns the string representation, and Bool returns true for positive and false for negativ.

Tuple inference also yields the corresponding values of `maxBound :: (Bool, Int, Char)` with `(True,2147483647,'\1114111')`  

The multiple values represented with  `:t minBound` as `(Bounded a) => a` shows what we call polymorphic constant, as in multiple values representing the same thing. Such as `Num` with it's transformations between `Int`, `Integer`, `Float`, and `Double`

An interesting piece with Multiplication `:t (*)` shows `(*) :: (Num a) => a -> a -> a` As in two of the same types must be present for the third type to be returned, so `(5 :: Int) * (6 :: Integer)` can’t work because we had defined two different types, but `5 * (6 :: Integer)` works fine since 5 can be all types of `Num`.

`Integral` a typeclass we had not used yet accepts only `Int` and `Integers`. As is the name (whole)
`Floating` on the other hand takes in the two `Float` and `Double` as non “whole” types.

 And finally, `fromIntegral` moves an Integral to a general number, as some methods such as length returns `Int` and not `Num`. It looks like `fromIntegral :: (Num b, Integral a) => a -> b`.

