[<<< Table of contents](../README.md)

## Control structures

1. Single-line if statements are only ever allowed with surrounding braces.

    ```cpp
    if (some_flag)
    {
        do_something();
    }
    ```

    This kind of code is not allowed at all:

    ```cpp
    if (some_flag)
        do_something();
    ```

    - <small> The former style provides greatly increased readability. Brace-related programming errors (or errors creeping in through code merges) are avoided. It’s too easy to “just add another line” without adding braces, and then days later follow up by debugging some weird error (that was caused by this error in judgement) for hours. Also, remember <a href="http://www.dwheeler.com/essays/apple-goto-fail.html">`goto fail; goto fail;`</a>?</small>

2. Any kind of assignments inside `if` statements are not allowed. Always split the assignment to a variable and a check against the variable over two lines.

    Example:

    ```cpp
    if (f = some_func())    // not allowed
    {
        // ...
    }
    ```

    Instead, write:
  
    ```cpp
    f = some_func();
   
    if (f)
    {
        // ...
    }
    ```

    - <small>Don’t try to be too clever. An assignment is not what one would usually expect inside an `if` statement.</small>

3. In `if` statements, Yoda conditions (i.e. writing `(<constant> == <variable>)`) are explicitly not allowed. We write the human readable `if (var == 2)` instead of the much less readable `if (2 == var)`.

    - <small>Instead of making code unreadable by introducing funny-looking equality comparisons, a much better way to protect oneself against accidental assignments (`if (var = 2)`) is to increase the warning level, and to compile cleanly at the highest warning level. All decent compilers will then emit a warning whenever an assignment inside an `if` statement is made. Make sure this warning is enabled.</small>

4. `case` statements that contain several statements inside or contain any local variables should be wrapped with braces. Cases that contain just one statement may be written on one line with the `case` label (and `break` statement, unless the case only consists of a `return` statement).

    The `break` statement should be placed inside the brace scope of each `case` label, so that the only statements outside of the respective brace scopes are the `case` labels themselves.

    Example:

    ```cpp
    switch (some_value)
    {
        case 0: return 123; // this is a single-line case
    
        case 1:
        {
            int a = b; // this is a multi-line case
            b = c;
            break;
        }

        case 2:
            do_something(); // this style is not allowed; use braces
            break;

        case 3: int a = b + 1; return a; // also not allowed

        default: break; 
    }
    ```

5. Avoid using fall-through cases (i.e. no `break` or `return` at the end of a `case` statement). Always provide either a `break` or a `return` statement at the end of each `case` statement. Code lacking either of these statements will be considered erroneous.

6. Add `default: break;` to the end of each `switch` statement that doesn’t consider all possible cases. This will avoid a compiler warning.
    
---

[<<< Table of contents](../README.md)