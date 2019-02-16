[<<< Table of contents](../README.md)

## Variables

1. Always declare variables as locally as possible. Do not declare or define variables before they need to be declared/defined. C89-style variable declarations, where all variables are declared at the beginning of a function, are not allowed. 
In particular, define for-loop variables inside the for-loop if they are not used outside of the loop body.

    - <small>Sometimes, hoisting a variable outside of a scope can provide a valuable optimization.</small>

2. Always initialize variables at the point of definition, unless there is a very compelling (performance) reason not to so do.

3. Only ever declare one variable per line. Avoid placing multiple variable declarations on the same line.

    Example:
  
    ```  cpp
    int x = 0;
    int y = 0;
    ```

    Not:

    ```cpp
    int x = 0, y = 0;
    ```

    - <small>This greatly enhances code readability.</small>

    - <small>It also fixes the unfortunate language defect inherited from C, where `int* p, q;` declares one pointer and one non-pointer variable. Obviously, writing `int* p, *q;` or `int *p, *q;` should be avoided, as it would break the rule above.</small>

4. Make use of automatic type deduction (via the `auto` keyword) wherever it makes sense. Aim to prefer using `auto` over explicit type declarations, but feel free to explicitly specify the type whenever it helps readability.
  
    Specify the type when explicit conversions are desired. In this case, consider using the left-to-right auto syntax (`auto var = Type(init);`) over the old-style syntax (`Type var(init);`). The former is more uniform, considering function return syntax and the `std::make_*` helper functions.
  
    These examples illustrate the consistency argument:
  
    ```cpp
    auto int_value = 42;
    auto fp_value = 42.0f;
    auto e = Employee(id);
    auto widget = get_widget();
    auto w = std::make_unique<Widget>(1, 2, 3);
    ```

    We still consider the old-style syntax more readable for the definition of fundamental and some custom types, so this should be preferred:
  
    ```cpp
    int value_i = 42;
    float value_f = 42.0f;
    Employee e(id);
    ```

    Use `auto` if you would otherwise have to repeat a type:
  
    ```cpp
    auto w = std::make_unique<Widget>(1, 2, 3);
    // instead of:
    std::unique_ptr<Widget> w = std::make_unique<Widget>(1, 2, 3);
    ```

    Also, prefer using `auto` in for-loops (range based or otherwise), to not have to specify elaborate type names:
  
    ```cpp
    for (auto itr = vec.begin(), itr_end = vec.begin() + n; itr != itr_end; ++itr) ...
    for (const auto& p : my_map)
    ...
     ```

    - <small>There is simply no need to explicitly specify a type in most cases; the compiler knows the type, often better than the programmer. Most importantly, automatic type deduction guarantees that no implicit conversions take place. This guarantee is not given with explicit type declaration, where the type might be wrong.</small>

    - <small>Judicious use of auto makes code more readable, because complex type declarations can be omitted. It makes code also more maintainable, since type changes are automatically tracked and do not have to be done manually throughout the source code.</small>
     
    - <small>Do not use `auto` in some very specific cases, e.g. with proxy types or expression template libraries. For more details, refer to Scott Meyers, "Effective Modern C++", Item 6.</small>
    
---

[<<< Table of contents](../README.md)