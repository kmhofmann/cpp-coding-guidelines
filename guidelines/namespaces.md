[<<< Table of contents](../README.md)

## Namespaces

1. Namespaces provide the proper means for code modularization and a means for logical grouping/decoupling of components. They specify programmer intent and help prevent polluting the global namespace.
 
    All library code should be part of a namespace, which is either a company-wide or library-wide identifier.

2. Consider using a fixed top-level namespace per library, and a sub-namespace for each sub-module inside the library.

    Example:
  
    ```cpp
    // file A:

    namespace foo
    {
    namespace img
    {
    
        // image processing stuff

    } // namespace img
    } // namespace foo
    ```
    ```cpp
    // file B:
    
    namespace foo
    {
    namespace math
    {
    
        // maths related stuff

    } // namespace math
    } // namespace foo
    ```

3. Repeating the namespace statement as a comment after the closing brace (see example above) allows quick visual matching of braces.

    Example:
  
    ```cpp
    namespace foo
    {
    } // namespace foo
    ```

4. The names of namespaces should be short, meaningful, concise, and all lower-case. Try to avoid use of underscores inside namespace names.

    Good namespace names are, e.g. `img`, `math`, `util`, etc.

    Bad namespace names are, e.g. `Utilities`, `txwz`, `my_nmspc`, etc.

    - <small>An exception to this rule can be a sub-namespace `namespace Private`, or `namespace Impl`, where the uppercase usage signals that all content is to be treated as implementation detail, not a public interface.</small>

5. Never ever use a `using namespace` statement at global or namespace scope in a header file.

    This is strongly discouraged, since it would pollute the global or respective namespace for every file that includes said header file.

6. Use of `using namespace` statements (except for `using namespace std;`!) is generally permitted inside translation units, as long as the likelihood of ambiguity or naming clashes remains low, and readability remains preserved. If in doubt, explicitly prefix the namespace.

    Inside translation units, prefer using the `using namespace` statements at function scope instead of global scope.

7. Never write `using namespace std;`, even in translation units. All uses of standard library components should be explicitly prefixed by `std::` to make it clear that a standard component is being used, and to [prevent creation of name conflicts and ambiguities](https://isocpp.org/wiki/faq/coding-standards#using-namespace-std).

8. Do not use any kinds of prefixes to indicate membership of constants or functions to a library or module. This might have been a bad, but necessary "hack" in C code, but is considered bad style in C++ code. Namespaces provide a much better alternative.
    
---

[<<< Table of contents](../README.md)