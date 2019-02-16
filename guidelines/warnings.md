[<<< Table of contents](../README.md)

## Compiler warnings

1. All code should compile cleanly without any compiler warnings at the highest warning level. Disabling important warnings is not allowed, except selectively for third-party libraries.

    - <small>In almost all cases, if the compiler emits a warning, there is something wrong with the code. Write clean code, instead of ignoring or even disabling the warning.</small>

2. Use the `-Wall -Wextra` flags in GCC and Clang to enable most warnings, and enable more if deemed necessary.

    Consider using the `-Weverything` flag in Clang to enable *all* warnings (a flag absent from GCC), and then explicitly disable the ones deemed unimportant.
  
    Use the `/W4` flag in Visual C++.
    
---

[<<< Table of contents](../README.md)