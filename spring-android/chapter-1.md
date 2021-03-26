# An overview of building native apps using Spring and Android

A couple of basic reminders.

## Variables

Also known as fields in Java, 4 types are what defines each from the other:

- Instance Variables: is what an instance also infers, a unique field even within a common class.

- Class Variables: is defined by adding a `static` keyword before decclaring that variable, and is common/accessible to all objects of that class.

- Local Variables: are usually restricted in scope, mostly within the method its defined in.

- Parameters: are instead defined as what is inputed in certain functions and methods and should be handled accordingly.

Naming conventions in Java dictates no use for `$` and `_` as part of the variable naming, unless its a constant value preceded by a `final` such as `static final int NUM_GUESTS` which also contains capitalized words, otherwise stick with the `camelCase` convention!

## Operators

Precedence is an important concept to be reminded of, generally it goes like:

```list_items
--> postfix [expr++]
--> prefix [++exp] && unary [~,!]
--> multiplicative [*,/,%]
--> additive [+,-]
--> shift [<<,>>,>>>] **New concept, byte shift
--> relational [<,>,<=,>=, instanceof]
--> equality [==, !=]
--> btiwise AND [&] **New concept, byte operator
--> bitwise Excl. OR [^]
--> bitwise Incl. OR [|]
--> logical AND [&&]
--> logical OR [||]
--> ternary [?,:]
--> assignment [=,+=,-=,*=,/=,%=,&=,^=,|=,<<=,>>=,>>>=]
```

[**New concept is explained well in this website.](https://www.iitk.ac.in/esc101/05Aug/tutorial/java/nutsandbolts/bitwise.html)
Otherwise, pretty standard in terms of precedence and priorities.

## Expressions, Statements and Blocks

Lets take some english/comupter lessons.
Expressions: Its when something is evaluated, and is done so with single operations, such as `assignment`, and could be compounded as long as data types matches.
Statements: Just like full sentences, a statement is a bunch of expressions ended with a semicolon `;`.
Blocks: When statements hang out together inside curly rbaces `{}`, they rarely interact with the outisde, unless specified otherwise.

## Control Flow Statements

Most of them such as (`if-then-else`, `switch`, `for`,`do-while`, `break`,`continue`,`return`), have been dealt with before as a javascript developer. Continue however had small edge case uses, and is very useful to skip loops but not break from it.

### Compiling

Human language code to byte code really. A subtle difference when talking about runtime vs compilation error, where the former happens after the program is already being used, while the latter happens while the machine is still trying to understand and run it the first time.

### The Docs of Java Oracle

Now [this might look intimidating](https://docs.oracle.com/javase/8/docs/api/), but it has some standard procedures you could check out in order not to get lost in the pages. A [dummies guide](https://www.dummies.com/programming/java/making-sense-of-javas-api-documentation/) might cut it to manageable pieces amirite?

The main take when looking at the docs for the first time, is its class system definitions, things start out using the more generic classes, into their methods and finally each use case. Just like what a tree would look like, another useful note is the `overloaded` methods, where a single method would be defined multiple times to accomodate different arguments and perhaps implementations.
