# Clean Code

#### First Edition || By Robert C. Martin

## Overview

_Clean Code_ outlines the tenets of writing readable, efficient, and maintainable code. The book provides examples of many flavors of "bad code" and the problems it can cause, and explains how to transform it into the "clean code" that developers prefer.

Topics span from choosing names for variables, to writing and formatting functions and classes, to designing system architecture. The overarching theme is one of software as a craft - like any other craft, the product is best when each step in the creative process is taken carefully and consciously.


## Table of Contents <a name="toc"></a>
+ [Chapter 1: Clean Code](#ch1)
+ [Chapter 2: Meaningful Names](#ch2)
+ [Chapter 3: Functions](#ch3)

## Chapter 1: Clean Code <a name="ch1"></a>

[(return to table of contents)](#toc)

#### On the Costs of Bad Code

+ Bad code leads to more bad code, causing productivity to slow as the codebase becomes increasingly unwieldy
    + Changes are seldom trivial to make in a disorganized system, and often result in a greater mess
    
#### On the Benefits of Clean Code

+ Keeping code clean at all times is generally faster than hacking together a mess to meet a deadline
    + Continuous refactoring and conscious design choices are likely faster than large overhauls later on
+ Code gets read much more than it gets written
    + Even the code's author repeatedly reads over their own code, while writing related code

### Quotes

> Clean code does one thing well.

> One broken window starts the process towards decay.

> You know you are working on clean code when each routine you read turns out to be pretty much what you expected.


## Chapter 2: Meaningful Names <a name="ch2"></a>

[(return to table of contents)](#toc)

#### Bad Naming Habits to Avoid

+ Choosing names that convey no information, such as `int d` or `List myList`
+ Using "magic numbers" (which lack names entirely) - replace them with constants or accessor methods given meaningful names
+ Using "number series" naming, such as `a5 = (a1 == a2) ? a3 : a4;`
+ Including meaningless/redundant "noise words", such as `clientObject` (versus simply `client`), or `fullNameString` (versus just `fullName`)
    + If there's no obvious difference between two names (like `getCustomer()` and `getCustomerData()`), it'll just lead to confusion
+ Encoding type and/or scope information in names; this is often unnecessary noise, given modern languages and tools
+ Using multiple words for one concept within a scope
    + E.g., using some combination of `add`, `push`, and `enqueue` in a `Queue` interface would be very confusing
+ Using one term to express two or more different concepts
    + E.g., using `add` to mean both a) creating a new value via concatenation of two others, and b) inserting an element into a collection
    
#### Good Naming Habits to Emulate

+ Choosing names that express what the named object does, why it exists, and how it's used
+ Making names pronounceable, so they can be easily discussed ("without sounding like an idiot")
+ Making names searchable, so they're easy to locate across a codebase
    + Searchable names are descriptive and reasonably unique
    + Predictable casing conventions also aid searchability
    + Constants and very short names are the hardest to search for
+ Replacing overloaded constructors with more descriptive factory methods
+ Using nouns for class/object names, and verbs for method names
+ Using solution domain names (i.e., terms like `Queue` that other programmers will understand) when possible
    + Otherwise, use the problem domain names
    + Avoid making up your own terms whenever possible
    
#### Other Notes on Naming

+ Descriptive names remove the burden of mentally mapping from a vague name to the actual concept
+ The context of a name contributes to its meaning - `Address.state` has a very different meaning than `Process.state`
    + If prefixes are needed to group certain names together, they might be best moved to their own context, with a name expressing the prefix's meaning
+ Names should be just long enough to be clear and descriptive
    + Avoiding redundancies helps prevent overly long names

### Quotes

> The length of a name should correspond to the size of its scope.

> If you can’t pronounce it, you can’t discuss it without sounding like an idiot.

> ...if you can reliably remember that r is the lower-cased version of the url with the host and scheme removed, then you must clearly be very smart.


## Chapter 3: Functions <a name="ch3"></a>

[(return to table of contents)](#toc)

#### Function Names & Bodies

+ Function names should be descriptive, even if that means being somewhat long
+ Functions should be small
    + Not too many lines (ideally, less than 20)
    + Not too long of lines
    + No deeply-nested structures
+ Short, clear functions can be achieved by refactoring related blocks within the function into their own descriptively-named functions
+ Functions should only do "one thing"
    + If it's possible to divide a function into sections (which could be their own meaningful functions), it might be doing more than "one thing"
+ Functions should handle only one level of abstraction
    + I.e., don't mix low-level details with higher-level function calls
    + Corollary: reading down a list of function calls should correspond to progressively lower levels of abstraction
+ Functions should avoid side effects; everything they do should be predictable from their name

#### Function Arguments

+ The number of arguments to a function should be minimal
    + The argument count can sometimes be reduced by grouping related arguments into an object (e.g., `Date` instead of `int year, int month, int day`)
+ Outputting information through an argument (e.g., by mutating it) can be confusing; the expected behavior is often for information to be output through the return value
+ Some terms: functions that take zero, one, two, and three arguments are called niladic, monadic, dyadic, and triadic, respectively
+ Be wary of creating argument lists of two or more with no obvious positional relationship; it's difficult to remember which argument goes where

#### Other Notes on Functions

+ Lumping together commands and queries within a function means the function is doing more than one thing, and can be confusing
    + This is often seen as returning an error code if the "command" portion failed to execute
    + Instead, commands and queries can be put in separate functions: `if (query()) { command(); }`
+ Exceptions are preferable to error codes
    + Consider moving the contents of try/catch blocks to their own functions
+ Don't Repeat Yourself (DRY) - avoid duplicated code at all levels, including inside/between functions
    + If a change must be made to duplicated code, it has to be made multiple times

### Quotes

> Functions should do one thing. They should do it well. They should do it only.

> Duplication may be the root of all evil in software.

