# Looping over complexity

As with many projects, first step involves breaking up your problems to workable mini-problems, and by workable as in maintable, readable and is easy to modify by many programmers over the lifetime of that code!

## Importing Importance

First steps include file structure, and with files and folders come imports to weave them together. Many of which would come as part of core java packages, which are ready made for you to use.

An example mentioned in [programiz.com](https://www.programiz.com/java-programming/packages-import) is about an SQL interface called `ResultSet` which belongs to the `java.sql` package which "*includes all types that are needed for the SQL query and database connection*".

Unique names for classes are important in Java, duplications only happen if they were imported seperately using two different packages.

### Built-in Goodness

`java.lang`, `java.util`, `java.io` are of those that comes from within the JDK. `ArrayList` is heavily used and is within the `java.util` package.
Import is done as following:
`import java.util.ArrayList;`.

### Your own Goodness

Another way to import packages, is to make them yourself first. That is done by adding the first line in a `file.java` file with `package packageName` that will now encapsulate all classes to that filename.

The package name is not arbitrary but follow the file system pattern starting from root `com` folder or any other, such as files such as `com` -> `test` -> `Test.java` would have a package named as `package com.test;`.

Naming conventions dictates a pattern that looks like a web domain, but in reverse such as `com.company.name`.
Once packaging is out of your concern, much of the file system complexity becomes clear to work with.

## Start Looping

But first, lets get to know the difference between Arrays and ArrayLists:

- Arrays: Standard js array, with a couple of limitations
  - Cant increase once decclared.
  - Cant have more than one data type.
- ArrayList: Arrays `2.0` really with flexible length, but still can't have more than one data type.. plus an import and funny declaration `ArrayList<T> arr = new ArrayList<>();`

Otherwise these stuff we have already had dealt with before!
