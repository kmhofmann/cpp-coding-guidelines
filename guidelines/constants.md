[<<< Table of contents](../README.md)

## Constants/Literals

1. Constants are to be defined as `constexpr` or `const` variables, not as preprocessor defines. Using `#define` to define constants is strongly discouraged.

    - <small>The preprocessor is just a stupid text replacement tool and should be avoided at all cost. It is completely unaware of type safety/type checking and not obeying scoping. “Defined” constants are not visible in the debugger, their address cannot be taken, they cannot be passed by const-reference if needed, and they do create new “keywords” when put into header files.</small>

    - <small>C++ provides vastly better tools for any imaginable use of `#define` outside of header include guards or conditional compilation.</small>

2. Consider putting integer constants into either an `enum class` (not an old-style `enum`; see below) or a (potentially type-specialized) class if they logically belong together.

3. Avoid global constants (outside of namespaces) by all means, as it leads to name conflicts. Instead place static `const[expr]` definitions into classes, or use anonymous namespaces inside translation units.

    Inside translation units, do not place constants outside of (anonymous) namespaces, since then other translation units could still access these variables via the `extern` keyword. (Needless to say, avoid any use of the `extern` keyword in this context.)

4. Numeric literals are to be specified by their precise type; implicit conversions by the compiler are not acceptable (unless unavoidable).

    The canonical form for floating point numbers has to be human-readable, i.e. omitted digits after the decimal point while providing the decimal point are not allowed.
  
    Example:
  
    ```cpp
    float x = 1.0f;
    double x = 1.0;
    long x = 1l;
    unsigned long x = 1ul;
    ```
  
    Do not write:

    ```cpp
    float x = 1.0;        // should be 1.0f
    float x = 1.f;        // should be 1.0f
    double x = 1.0f;      // should be 1.0
    double x = 1.;        // should be 1.0
    long x = 1;           // should be 1l
    unsigned long x = 1l; // should be 1ul
    ```

5. To specify numeric literals of a certain type, preferably use the literal suffix. Otherwise use the initializer form. Do not use C-style casts.

    Example:
  
    ```cpp
    5l
    42ul
    unsigned char(16)
    short(100)
    ```
    
    Do not write:
  
    ```cpp
    (long)5
    (unsigned char)16
    (short)100
    ```
    
---

[<<< Table of contents](../README.md)