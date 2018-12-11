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
+ [Chapter 10: Classes](#ch10)
+ [Chapter 11: Systems](#ch11)
+ [Chapter 12: Emergence](#ch12)
+ [Chapter 13: Concurrency](#ch13)
+ [Chapters 14 - 16: Case Studies](#ch14-ch16)


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


<a name="ch9"></a>
## Chapter 9: Unit Tests

[(return to table of contents)](#toc)

+ Test-driven development (TDD) implies that tests should be written along with production code
    + Specifically, a minimal failing test should be written, then minimal code to pass that test (and previous tests) should be written, and then iterate
+ A messy test suite results in the same maintainability problems as messy production code
    + If the messy test suite is simply discarded, then developers have no way of verifying that changes don't break earlier functionality
+ Tests are not an exception to "code is read more than it is written"
+ Testing code is less constrained by efficiency than production code, so long as it doesn't take prohibitively long to run the testing suite
+ Minimizing the number of `assert` statements per test keeps tests simple to read
    + Each test should test one (minimal) concept only; if this requires multiple `assert` statements, it's okay
+ Tests should be _fast, independent, repeatable, self-validating, and timely_ ("FIRST")


<a name="ch10"></a>
## Chapter 10: Classes

[(return to table of contents)](#toc)

+ Classes should declare variables first: constants, followed by private static variables, followed by private instance variables
+ Public methods come after variable declarations, with any private utility functions they use grouped after the calling function
+ Classes should be small - like functions, they should be limited to a single responsibility
    + Size generally self-limits once a class is limited to "one responsibility"
+ Classes should be _cohesive_, meaning that most functions within the class need most of the instance variables to operate
    + If a subset of functions is using a subset of the instance variables particularly often, while other functions aren't, then those functions and variables are a candidate for their own class
+ Classes should be designed to minimize the risk of breaking existing functionality when changes are needed
    + This ties in well with the single responsibility principle and small classes, which minimize the coupling of code
    + Ideally, new features should be extensions to the system, not modifications to existing classes
+ Decouple class dependencies by formalizing/abstracting them using an interface, then hiding specific implementations behind that interface
    + This approach can help when code is being tested, as well as being changed
    + The given examples is a `Portfolio` class that depends on a `TokyoStockExchange` API; instead, it can be written to depend on a new, more abstract `StockExchange` interface, which can be implemented by the `TokyoStockExchange` or some stock exchange class made explicitly for testing `Portfolio`


<a name="ch11"></a>
## Chapter 11: Systems

[(return to table of contents)](#toc)

+ The startup logic of an application should be decoupled from its runtime logic
    + This can be done by moving all startup construction tasks to `main()`, and hiding that logic from the rest of the system
+ _Revisit the rest of this chapter_


<a name="ch12"></a>
## Chapter 12: Emergence

[(return to table of contents)](#toc)

+ This chapter focuses on four rules of simple design (due to Kent Beck) that facilitate the emergence of clean structures
+ _Rule 1:_ Runs all the tests
    + Systems must function as they're intended; otherwise, how "clean" or "simple" they are doesn't really matter
    + Continuous testing (TDD) may expose excessive coupling or complexity in the code, since tests are harder to write for convoluted code
    + For example, if a class/method has "more than one responsibility", it will quickly become apparent that two separate functions are being tested for one entity
+ _Rule 2:_ Contains no duplication
    + Similar/identical lines of code are obvious candidates for refactoring
    + Functions with similar implementations may contain duplication; perhaps refactor one to use the other, or extract the shared implementation details into a third function that both other functions call
    + Higher-level duplication may require new class inheritance schemes, abstract classes, or interfaces
+ _Rule 3:_ Expresses the intent of the programmers
    + Much of this book has already dealt with ways to make code expressive: choose descriptive names, write small functions/classes, and include a test suite that clearly demonstrates desired behavior
+ _Rule 4:_ Minimizes the number of classes and methods
    + Creating a class or method for every possible bit of code that could possilby be its own class or method ends up defeating expressiveness and simplicity, at a certain point
    + On the other hand, having too _few_ classes and methods (especially large ones) may indicate that they each have too much responsibility
    + Don't go overboard on "no duplication", "expressive naming", or any other design philosophies that lead to a counterproductive, excessively high number of classes or methods

### Quotes

> Duplication is the primary enemy of a well-designed system. It represents additional work, additional risk, and additional unnecessary complexity.


<a name="ch13"></a>
## Chapter 13: Concurrency

[(return to table of contents)](#toc)

+ Concurrency decoupled what is done from when it gets done
+ Concurrency addresses throughput and response time challenges, and may lead to a cleaner overall structure
    + A particularly common use for concurrency is to continue doing work while waiting at bottlenecks, such as IO operations
+ Concurrency leads to some overhead (development time and execution time)
+ Concurrency introduces significant complexity, even for simple systems
+ Concurrency bugs are generally hard to repeat
+ Code with concurrent logic should be separated from single-threaded code
+ Data shared between threads should be very limited in scope and means of being accessed/updated
    + The more critical sections are spread throughout the code, the more they're likely to be mishandled
    + Consider making read-only copies when shared data is accessed, then merge modifications in a single thread with write-access
+ The more independent different threads are (i.e., the less data they share), the less likely concurrency bugs are
+ _Revisit the rest of this chapter_


<a name="ch14-ch16"></a>
## Chapters 14 - 16: Case Studies

[(return to table of contents)](#toc)

+ These chapters present three in-depth, code-heavy case studies in refactoring code; they are not well-suited for notes, so they're omitted here


<a name="ch17"></a>
## Chapter 17: Smells and Heuristics

[(return to table of contents)](#toc)

+ Most of the items in this chapter are covered in more detail in previous parts of the book; these aren't repeated here
+ A few code smells unique to this chapter:
    + _Clutter:_ unused functions, variables, empty constructors, and so forth should all be removed
    + _Feature envy:_ excessive reliance on, or manipulation of, another class's data; this is generally far too much coupling
    + _Misplaced responsibility:_ constants, functions, and the like should be placed with whatever code other readers would most expect them to be near
    + _Inappropriate static:_ when making a member static, think about whether there's any reason it may need to be polymorphic in the future; if so, don't make it static
+ A few guidelines unique to this chapter:
    + _Encapsulate conditionals:_ boolean tests are easier to understand if they're extracted to a method whose name describes the intent of the test
    + _Avoid negative conditionals:_ remembering a "not" for one expression in a conditional requires extra mental work
    + _Encapsulate boundary conditions:_ `str.length() + 1` repeated over and over is hard to read, especially if mixed with `str.length()`; instead, store boundary conditions in a variable like `int end = str.length() + 1`
    + _Keep configurable data at high levels:_ don't bury user-configurable values deep within the program; handle them up-front, preferably in `main()`
+ Java-specific guidelines:
    + Prefer import wildcards to long import lists
    + Don't import constants through an arbitrary, non-obvious interface; put them in a dedicated "constants" class and use a static import
    + Prefer enums to `int` constants; they're more centralized and powerful (i.e., they can defined methods and fields)
    