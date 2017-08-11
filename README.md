# Comments on Golang

## A point of view on Golang's pros and cons

* The Go language is relatively simple, making for a shorter learning curve than most other powerful languages.
* The "batteries included" philosophy, with a rich set of standard tools out-of-the-box, contributes to consistency (which also helps with quality) and shorter learning curves.
 - This includes out-of-the-box coding style conventions and automated code formatting.  No more fruitless coding style debates.
 - The *go test* tool, including its support for example code, coverage, benchmarking, and profiling, is a powerful contributor to development productivity and quality.  In addition, Go's profiling API supports programmatic profiling of applications.
* However:
  - Lack of a REPL is very annoying, notwithstanding fast compilation and the existence of the *go run* command and the *Tour of Go* server (local or web)
  - While easy-to-use, standard go tool support for versioning of library dependencies is too simplistic.  There are ways to deal with this challenge but it is a source of pain for software version and release management.  The pain is compounded by poor support for symlinks by the standard go tools.
* Go strives for conciseneess, with type inferencing being a key support.
* The lack of inheritance is a big plus.  It prevents much of the confusion often associated with object-oriented designs.  Instead, Go supports rich data structures, functions as first-class objects, the decoupling of data and functions, the ability to define methods on data types, duck-typing interfaces, delegation, and a simple (but effective) approach to exporting names from packages.  In short, Go supports most of the benefits of object-orientation, with more concise syntax, while avoiding the traps associated with class inheritance.
* Interfaces in Go support efficient duck-typing in a statically-typed language, a very powerful feature.
* Idiomatic Go programming relies heavily on mutation and side-effects
  - Although Go supports functions (closures) as first-class objects, it is not practical to adopt a mainly functional programming style with Go
    - Lack of generics / parameterized types
    - Lack of reasonably efficient immutable collections
  - This is a regression with respect to modern function-oriented programming.  I consider functional programming the most effective paradigm for appropriate modularization of systems and also the most effective in reducing the gap between functional designs and code.  I see this as a major disadvantage of Go.
  - On the other hand, Go enables very elegant and efficient imperative programming.
  - Nonetheless, one can structure a Go application in terms of functions and use functions as the core unit of composability while using impure (state-mutating) functions and implementing functions in an imperative style.  This mixed paradigm can still be beneficial in reducing the gap between functional designs and code, producing code that is more understandable and maintainable.
* A simplistic, non-uniform, type system without generics limits the expressiveness of the language.
  - Can promote boiler-plate that may counter the language's conciseness characteristics.
  - Alternatively, can promote the use of type assertions or, worse, reflection.
  - Further drives coding with state mutations (e.g., functions taking an output parameter of an interface type instead of returning a value).
* Short variable declarations are a fine feature but it can be confusing as the same syntax can be used for multiple assignments involving previously declared variables so long as at least one is declared at that point.
* Type declarations, using the *type* keyword, are great.  They create new types based on an existing type.  They are different from the *type* keyword in Scala, which only defines a type alias.  In Golang, you get a new and different type, which happens to have the same in-memory footprint as the type from which it is defined.  This feature is similar to Scala's *extends AnyVal*, but additionally the new type inherits the methods and other features of the source type.
* The use of pointers is a throw-back, but can be tolerated.
* Direct support for UTF-8 in source code and strings is convenient and efficient, simplifies dealing with Unicode.
* Availability of drivers for databases is not as wide as for Java; there is no officially supported ODBC interface.
* Anonymous structs are a nice way to define nested data structures without having to name the types of sub-structures, providing a lightweight syntax similar to that of JavaScript objects.
* Go's default of zero values for types (including structs), as well as its treatment of typed nil values and the access to absent keys in maps, greatly simplifies code by avoiding boilerplate initializations.
* Go supports dynamically growing stacks to enable deep recursion without stack overflow.  However, it does not provide tail call optimization, thus reducing the attractiveness of recursion for high-performance Go code.
* The *defer* mechanism is a great way to deal with the safe usage and closing of resources.
* Go's error-handling philosophy is to promote handling of errors as close to the occurrence as possible.
  - This is a point of religion in language discussions (e.g., https://github.com/golang/go/issues/18721).
  - This may be fine for library code but is unfortunate for much business logic, where error-handling should be done at the root of the call stack and delegated to an architecture-provided last-resort error handler.
  - For common cases where errors are detected in business logic or architecture functions but should be allowed to bubble-up, we should establish a pattern (and helper types and functions) to force a *panic* and then handle the *panic* with a call to *recover()* in a last-resort error handler.
* Built-in support for the CSP concurrency model, through channels and goroutines, makes concurrent programming natural, efficient, and relatively easy in Go, especially as compared with thread-based concurrency in other languages.
* There is a perception that Go is more performant than Java, but that is dubious.  At this time, on many benchmarks, Java beats Go.
