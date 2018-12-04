# Clean Code

#### First Edition || By Robert C. Martin

## Overview

_Clean Code_ outlines the tenets of writing readable, efficient, and maintainable code. The book provides examples of many flavors of "bad code" and the problems it can cause, and explains how to transform it into the "clean code" that developers prefer.

Topics span from choosing names for variables, to writing and formatting functions and classes, to designing system architecture. The overarching theme is one of software as a craft - like any other craft, the product is best when each step in the creative process is taken carefully and consciously.


<a name="toc"></a>
## Table of Contents
+ [Chapter 1: Clean Code](#ch1)
+ [Chapter 2: Meaningful Names](#ch2)
+ [Chapter 3: Functions](#ch3)
+ [Chapter 4: Comments](#ch4)
+ [Chapter 5: Formatting](#ch5)
+ [Chapter 6: Objects and Data Structures](#ch6)
+ [Chapter 7: Error Handling](#ch7)
+ [Chapter 8: Boundaries](#ch8)
+ [Chapter 9: Unit Tests](#ch9)


<a name="ch1"></a>
## Chapter 1: Clean Code

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


<a name="ch2"></a>
## Chapter 2: Meaningful Names

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


<a name="ch3"></a>
## Chapter 3: Functions

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


<a name="ch4"></a>
## Chapter 4: Comments

[(return to table of contents)](#toc)

+ Writing code expressive enough to not require a comment is preferable to writing a comment
+ Comments are hard to maintain - and unlike code, failure to maintain them doesn't actually break the program (forcing corrective maintainance)
+ Comments can be inaccurate for a variety of reasons, confusing or misleading readers

#### Good Comments

+ _Legal comments_ - headers providing legal information, such as a license
+ _Informative comments_ - such as explaining a regex with an example pattern
+ _Explanation of intent_ - especially if something unorthodox is being done intentionally
+ _Clarification_ - such as providing a more readable equivalent of a standard library call
    + E.g., `// a == b` is simpler to read than `a.compareTo(b) == 0`
+ _Warnings of consequences_ - for the benefit of other programmers (used sparingly)
+ _TODO comments_ - just don't let them pile up too badly
+ _Amplification_ - underscoring the importance of something in the code that otherwise risks being tampered with (similar to _'Explanation of intent'_ above)
+ API documentation - users of the API rely on these, so include them

#### Bad Comments

+ _Mumbling_ - comments that make no sense to readers, and are more like the author's notes to themself
+ _Redundant comments_ - if the code is simple or self-explanatory, it doesn't need a comment
+ _Misleading comments_ - any comments that don't precisely match the behavior of the code
+ _Mandated comments_ - clutter resulting from rules that all functions/variables/classes must have complete documentation
+ _Journal comments_ - changelogs in the form of comments, which are  unneeded with modern version control systems
+ _Noise comments_ - comments that add no explanatory value to the code (e.g., `int today; // the current day`, or `// this is empty` in an empty `catch` block)
+ _Position markers_ - comments that delimit a certain section in the code (e.g., `// LOOK HERE //////////////////////////////////////`; use sparingly
+ _Closing brace comments_ - function logic should be simple enough that these are unneeded clutter
+ _Commented-out code_ - tends to stick around with no known intent; just remove it, and refer to the VCS if it's ever needed again
+ _Nonlocal information_ - comments should be kept close to the code they refer to; otherwise, they're almost impossible to maintain
+ _Too much information_ - keep comments short and to the point; maybe suggest some other resource where more information can be found
+ _Inobvious connection_ - it should be obvious how the comment and subsequent code relate

### Quotes

> The older a comment is, and the farther away it is from the code it describes, the more likely it is to be just plain wrong.

> ...the only truly good comment is the comment you found a way not to write.


<a name="ch5"></a>
## Chapter 5: Formatting

[(return to table of contents)](#toc)

#### Vertical Formatting

+ Files should not be too long; the author recommends 200 lines as a good average length, and 500 as a general upper limit
    + File length varies with project attributes, such as language used, so there are no hard-and-fast rules
+ Logical sections of the file should be separated by empty lines
    + E.g., package/module name, imports, fields, methods/constructors should all be separated
    + These separated sections should be "vertically dense" - open space within them ruins the impression of a consistent, interrelated block of code
+ Related sections of code should be vertically near each other
    + Closely interlinked functions, for example, should be grouped together - not at random locations within the file
    + When a function uses another function, the caller should be above the callee whenever possible
+ Variables in functions should be declared as close as possible to where they're used
+ Instance/class variables should be declared at the top of the class (this is purely convention)

#### Horizontal Formatting

+ Short lines are preferred, in terms of both readability and monitor size
    + Most organization's style guides set column limits of 80 to 120
+ Spacing around binary operators should reinforce the intent of the statement, making it easier to read at a glance
    + For instance, `a^^2 + b^^2 == c^^2` is preferable to `a^^2 + b^^2==c^^2` (and far preferable to `a ^^ 2+b ^^ 2==c ^^ 2`)
+ Standard hierarchal indentation for the given language should be followed
    + Code indented in a non-standard way, or not indented at all, is much harder to read
+ It's generally counterproductive to condense conventionally multiline constructs (`if`, `while`, `def`, etc) to one line
    + These cause readers to do a double-take, in exchange for a fairly unimportant reduction in total lines of code

#### Formatting Rules

+ That a project has a cohesive set of formatting rules followed throughout is more important than any one team member's formatting preferences
    + Mismatched formatting looks unprofessional and will annoy, if not confuse, readers


<a name="ch6"></a>
## Chapter 6: Objects and Data Structures

[(return to table of contents)](#toc)

#### Implementation Hiding

+ Design the interfaces of objects and data structures to hide their concrete implementations from users
    + Express the data in abstract terms, not just in simple getters/setters that essentially expose the implementation
    + Methods corresponding to abstract behaviors are more intuitive than those that force users to think about lower-level details

#### Objects vs Data Structures

+ Objects hide data and expose functions that operate on that data; conversely, pure data structures expose data and provide no functions for operating on it
    + Hybrids between these two are possible; they should preferably be kept separate, perhaps by wrapping the data structure within the object
    + A class with getters and setters for all its "private" fields still exposes its data, and isn't really behaving as an object (which should expose abstract behaviors via methods, not just data manipulation)
+ Objects make it easy to add new kinds of objects without changing existing functions (polymorphism), but hard to add new functions (because all existing objects must be changed)
    + _Note: In practice, Java addresses this by allowing `default` methods in interfaces, and concrete methods in `abstract` classes_
+ Data structures make it easy to add new functions without changing existing data structures (because they have no functions!), but hard to add new data attributes (because all outside functions using the structure must be changed to incorporate the new attributes)
+ "Data transfer objects" are data structures with no functions whatsoever, like `struct` in C; they serve only to encapsulate data (often as a bridge between database records and application code)


<a name="ch7"></a>
## Chapter 7: Error Handling

[(return to table of contents)](#toc)

+ Exceptions are preferred to functions that return error codes (or set error flags)
    + Error codes returned to calling functions must be handled in that function, cluttering the logic
    + Logic in a `try` block can be extracted to its own function, allowing separation of concerns (normal flow vs exceptional flow)
+ Checked exceptions (`throws...` in a Java method declaration) break encapsulation, since calling methods then know some implementation details of the methods they call
+ Error messages in exceptions should be informative
+ Returning `null` leads to the possibility of null pointer exceptions, which must be caught or prevented with checks for `null`
    + Instead, consider returning a null-like value, such as an empty collection
+ Passing `null` into a function creates a burden to check for and deal will `null` arguments
    + It's best to avoid ever passing `null` as an argument (i.e., modify the logic of the calling function to avoid this)
    + _Note: while it's certainly cleaner-looking when the callee doesn't have to have to check for `null` arguments, it requires a lot of trust in the caller, decreasing robustness_


<a name="ch8"></a>
## Chapter 8: Boundaries

[(return to table of contents)](#toc)

+ "Boundaries" here refers to the points of interaction between APIs created by someone else and code written by the developer
+ One way to hide unneeded/unwanted parts of an API being used it to simply encapsulate it in a custom class with the desired API
    + Similarly, this can be implemented as an "adapter" class between the desired interface and the actual one
    + This also facilitates easy changes to implementation details, such as swapping out an underlying data structure for a different one
+ Writing unit tests for proper usage of an interface is one way to learn a third-party API in a controlled way
    + This separates the challenge of integrating the third-party API with existing code for the project that requires it from that of learning the API
+ Clean, well-defined boundaries that insulate the system from implementation details and changes in third-party APIs are preferable to tightly coupled dependencies
    + Rewriting/refactoring a custom interface that serves as a bridge between the project and a third-party API is _much_ easier than locating and rewriting each API call throughout the project
