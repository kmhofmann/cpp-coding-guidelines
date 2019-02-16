[<<< Table of contents](../README.md)

## Comments

1. Aim to comment your code properly.

    Write both higher-level comments, as well as more detailed lower-level comments wherever appropriate. Any technically skilled colleague should be able to understand what is going on from both code as well as comments. Start-up costs related to code understanding should be kept to a minimum by commenting judiciously.

2. On the other hand, do not bury your code in comments. Omit any obvious comments (`++i; // increments i`). Well-written code should be largely self-documenting through choice of good variable and function names, logical program flows, etc.

3. All comments should be written in C++-style (`//`), with a variant of the C-style (`/*`) being mainly reserved for Doxygen-style documentation (`/**`).

    Always follow with a whitespace after starting a comment.
  
    Example:

    ```cpp
    // This is a proper comment.
    //Do not omit the whitespace.
    ```
    ```cpp
    /* Don’t write cumbersome C-style comments either. */
    ```
    ```cpp
    /**
     * @brief It’s ok and even encouraged for Doxygen-style documentation.
     */
    ```
     ```cpp
    /// This is also Doxygen-style documentation.
    ```

    - <small>C-style comments can not be nested; C++-style comments can be (per line), and many IDEs support automatic commenting or uncommenting of code blocks. </small>

4. When providing Doxygen-style documentation, place it directly above the definition of a function/member function, not the initial declaration either inside or outside of a class.

    - <small>Doxygen-style documentation is meant to be read in a browser, not inside the code. Any reader of your class should be able to get a quick overview by looking at the public interface in-code, and the more of the class fits onto a screen, the easier it is to read. Do not blow up the size of your class definitions by providing in-line documentation.
Also, this is one more reason to cleanly separate function declaration from definition (see also below, in ‘Classes’ section).</small>
    
---

[<<< Table of contents](../README.md)