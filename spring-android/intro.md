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
