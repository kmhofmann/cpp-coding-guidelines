[<<< Table of contents](../README.md)

## Macros

1. The use of macros is strongly discouraged. Avoid macros at all cost.

    - <small>The preprocessor is just a stupid text replacement tool and should be avoided at all cost. It is completely unaware of type safety/type checking and not obeying scoping. Defined macros are not debuggable and sooner or later will lead to nasty side effects (e.g. multiple increment problem).</small>

    - <small>C\+\+ provides vastly better tools for any imaginable use of macros; use inline functions or lambdas instead. The use of `#define` outside of header include guards or conditional compilation has no place in modern C\+\+ programming.</small>

    - <small>Exception: A macro can still be useful (and the only way, unfortunately) to implement a custom assert() facility, when using the predefined macros __FILE__ and __LINE__.</small>

---

[<<< Table of contents](../README.md)