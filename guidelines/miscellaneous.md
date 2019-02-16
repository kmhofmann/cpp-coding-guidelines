[<<< Table of contents](../README.md)

##  Miscellaneous

1. Avoid use of conditional compilation (`#ifdef`, or `#if defined()`), unless there is a very good reason for doing so (e.g. a NEON or SSE code path in-line).
If you have to use conditional compilation statements, avoid nesting them.

    Under absolutely no circumstances use conditional compilation in combination with multiple compilation of the same translation unit (a very poor template substitute in C).

    - <small>Conditional compilation can make code unreadable very quickly.</small>

2. Do not use multiple assignments within the same expression. Instead, place each assignment on a separate line in order to not decrease readability.

    Example:
  
    ```cpp
    char r, g, b;
    r = g = b = 0; // avoid

    r = 0; // more readable
    b = 0;
    g = 0;
    ```

3. When iterating through containers, use prefix form of increment on iterator objects. E.g. `++it`, **not** `it++`.
Use the prefix form also for counter variables in for-loops, etc., for reasons of consistency.

    - <small>The postfix form tends to create a temporary iterator object depending on code and compiler settings.</small>

4. If you choose to use the ternary operator (`?:`) in an expression, always wrap the sub-expression with parentheses. It is a source of many bugs because of its low priority in C++.

    Example: write `x = 10 + (flag ? 1 : 2)`. If you omit the parentheses in this case, you end up adding 10 to `flag` before the comparison.
  
    Consider avoiding the ternary operator in situations [where readability may be affected](https://isocpp.org/wiki/faq/coding-standards#ternary-op).

5. Use of the Pimpl pattern is generally discouraged. The pattern often adds a lot of unnecessary complexity and inefficiency, without providing great benefits.

    - <small>One exception case could be to separate a public interface (which will be used by a third party) from a company-internal implementation.</small>

    - <small>Another potential exception (and actually good use case) could be wrapper code for a C library, where it is important to keep the inclusion of the C header strictly inside a translation unit (for example because it overuses `#define` and/or pollutes the global namespace).</small>
        
---

[<<< Table of contents](../README.md)