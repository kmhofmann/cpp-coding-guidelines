[<<< Table of contents](../README.md)

## 1. The big picture

1. The language standard to be used is the current ISO standard, C++14.
(**NOTE: NEEDS UPDATING FOR C++17.**)
More modern language or library features should always be preferred over older, obsolete or less safe features or paradigms.

2. All features of the C++14 standard that are supported by the respective latest versions of the compilers used can and should be used whenever appropriate and useful.

     In case any code needs to be compiled by a not fully standards-conforming compiler (e.g. Visual C++ 2013 or 2015), refrain from using the unsupported features in the respective sections of the codebase.
  Modernize the codebase as soon as a new compiler version is released.

3. Refactor code whenever deemed necessary.
Do not let technical debt accumulate, since this will lead to unmaintainable code very quickly.
Beautify and simplify code whenever and wherever possible, since this will increase readability and maintainability.

4. Think in abstractions and focus on genericity and reusability.
If an algorithm or a data structure can be expressed generically without loss of performance, do not prematurely commit to only encoding a specific case. Alternatively, start with the specific case and genericize in the following step.
Avoid explicit duplication of algorithms or data structures; compilers are much better at doing this job (e.g. via template instantiation).

    - <small>In particular, consider putting any piece of code that is potentially reusable into an appropriate library. When doing so, ensure that your code fulfills the comparatively higher standards of library code, for example w.r.t genericity.</small>

5. Write correct and readable code first, micro-optimize later where necessary. Prefer elegant code over supposedly “hand-optimized” code that becomes obfuscated in the process. Optimize hot-spots in your code after judicious profiling.
On the other hand, do avoid premature pessimization. All other things, in particular readability, being equal, favor optimal code over pessimized alternatives.

    - <small>“Premature optimization is the root of all evil.” (Donald Knuth)</small>

    - <small>In many cases, programmers will severely underestimate the capabilities of the compiler optimizer, and therefore commit to inelegant, semi-obfuscated code a priori. This style of programming might have been acceptable in the ‘80s, but is not anymore today, including when targeting low-powered devices. Sometimes rigorous optimization is necessary after all, but in these cases profiling will prove the need. Know what your compiler optimizer can and cannot do.</small>
    
---

[<<< Table of contents](../README.md)