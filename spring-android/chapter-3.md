# Java Type System

Working out how statically typed languages work, as a dynamic language programmer could come off as an additional layer of restraints, but if you dealt with any substantial complexity in code, that restraint is a blessing in disguise. But that is not today's topic!

## Primitive vs Objects

Performance considerations in early Java development stages helps lend it the performance it boasts to have, such as thoughts on which to use, low cost primitives or the high costs reference objects. A quick [rundown into reality](https://betterprogramming.pub/node-js-vs-spring-boot-which-should-you-choose-2366c2f76587) before diving into the specifics does make it quite clear where Java excels.

All 8 data types `boolean 1bit`, `byte 8bit`, `char 16bit`, `short 16bit`, `int 32bit`, `float 32bit`, `long 64bit`, `double 64bit` have their equivelant reference objects that can be switched between in a process called unboxing and autoboxing. The main idea is depending on memory and processing needs its better to define what will be declared, otherwise if no limitation is needed this does not affect the program in any way.

The reference data type have a base memory footprint of `128bits` for most except for `long` and `double` taking a whooping `196bits` at declaration. Interesting performance benchmarks have noticeable effects, ranging from 1-4 times the time needed to compute. It might look like additional microseconds, but they add up fast, especially when [dealing with arrays](https://www.baeldung.com/java-primitives-vs-objects) of these types with hundred if not millions of them.

## Exceptional Exceptions and How to Catch Them

One ways to really differentiate good code, is how they try to handle errors that might be expected in error prone blocks, such as I/O code blocks and other places where the unexpected needs to be somehow put into consideration. Exception handling is the way to catch any errors and let the program continue regardless of that error, all thanks to the appriopriate error handling that matches the handler type and that of the error.

The main idea is to initialize error handling, so that the callstack is not disrupted during runtime and the error is forwarded instead to the said handler. A [comprehensive guide](https://docs.oracle.com/javase/tutorial/essential/exceptions/advantages.html) provided by Oracle has more exception related solutions.

To throw exceptions, one must bracket `{}` their problem with a `try` block, along with throw and catch statements in a way that honors the [Catch or Specify](https://docs.oracle.com/javase/tutorial/essential/exceptions/handling.html). The throw clause has a [number of ways](https://docs.oracle.com/javase/tutorial/essential/exceptions/declaring.html) to define different expections, very handy and a bit complex to start with.

Checked Exceptions, Errors and Runtime Exceptions are what you will encounter, the first is what you managed to deal very well, the latter two are inevitable and will cause the eventual demise of the program once it surfaces.

## [Error: More Comprehensive Summary Needed](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html);