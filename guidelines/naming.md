[<<< Table of contents](../README.md)

## Naming

1. There are generally two acceptable and internally consistent naming conventions.

    a) The "camel case" convention:
  
    ```cpp
    class AwesomeWidget;              // classes: upper-case & camel case
    double distanceFromPoint = 5.0;   // variables: lower-case & camel case
    double computeDistance();         // functions: lower-case & camel case
    double AwesomeWidget::getFoo();   // member functions: lower-case & camel case
    enum class State { CorruptData }; // enums: upper-case & camel case 
    ```
    Only class names start with an upper-case letter, whereas variable and function names start with a lower-case letter. All following syllables are directly connected to the preceding syllable and begin with an upper-case letter.

    b) The "underscores" or "snake case" convention:
  
    ```cpp
    class Awesome_widget;              // classes: begin upper-case & underscore separators
    class AwesomeWidget;               // some use camel case here (Python-like)
    double distance_from_point = 5.0;  // variables: lower-case & underscore separators
    double compute_distance();         // functions: lower-case & underscore separators
    double Awesome_widget::get_foo();  // member functions: lower-case & underscore separators
    enum class State { corrupt_data }; // enums: like classes; elements lower-case & underscore separators
    ```
    Only class names start with an upper-case letter, whereas variable and function names start with a lower-case letter. All following syllables are separated by an underscore (`_`) and keep the case of the preceding syllable(s).

    - <small>Use one of these naming conventions consistently throughout all related projects. Do not mix styles or deviate from the chosen convention.</small>

2. Class names and enumerations shall always begin with an upper-case letter, in order to cleanly distinguish them from (instance) variables. Never declare classes that begin with a lower-case letter.

    Example:
  
    ```cpp
    class Widget;
    Widget widget;
    
    enum class Level : unsigned char;
    ```

    - <small>Class and enumeration types are not variable names, and a visual separation is clearly desirable. Using uppercase identifiers for the former and lowercase identifiers for the latter is an unambiguous way to distinguish the two, and everyone immediately knows which one is which.</small>

    - <small>Constructions like</small>

      ```cpp
      widget my_widget; // funny name
      widget w; // name not expressive at all
      ```

      <small>will just lead to decreased readability at some point, and</small>
     
      ```cpp
      widget widget; // the obvious naming in many cases
      ```

      <small>is not legal code (unless the first letter of the class type becomes uppercase).</small>

    - <small>There shall be no `class` or `struct` name in the codebase that begins with a lower-case letter.</small>

3. Variable names shall therefore always begin with a lower-case letter.

    Example:
  
    ```cpp
    int index;
    Widget widget;
    ```
    - <small>There shall be no variable in the codebase that begins with an upper-case letter.</small>

4. Both member and non-member functions shall always begin with a lower-case letter.

    Example:

    ```cpp
    double compute_distance();
    const std::string& Window::name();
    ```

    Not:
  
    ```cpp
    double ComputeDistance();
    ```

    - <small>Function names are not class names, so prevent any possible confusion.</small>

    - <small>There shall be no function or member function in the codebase that begins with an upper-case letter.</small>

5. Any custom type names, e.g. in `typedef` or `using` statements, or in template declarations or definitions shall always begin with an uppercase letter.

    Example:
  
    ```cpp
    using Scalar = float;
    template <typename Border_policy> class Interpolator;
    ```

6. Use expressive, descriptive and meaningful names for variables and function arguments. Avoid names like `p`, `a`, `b`, `c` etc. and arbitrary abbreviations like `cntr`.

    One-letter variable names may be used in for-loops if they’re used as simple counters, but the use of a more meaningful name is explicitly encouraged.
  
    Example:
  
    ```cpp
    double f;       // this does
    std::size_t sz; // not help
    int i;          // readability
    ```

    ```cpp
    double factor;    // much
    std::size_t size; // better
    int feature_index; // :-)
    ```

    - <small>Good code should be self-documenting, not deliberately obfuscated. While there is a place for brevity (e.g. counter variables of some (but not all) for-loops), one should always err on the side of longer and more meaningful names. Remember, people besides yourself will have to read and/or work on your code, and be able to understand things quickly.</small>

7. For class template or function template declarations and definitions, use the `typename` keyword instead of the `class` keyword, since the former more accurately specifies what it is -- a type name. In the example below, `T` does not need to refer to an actual class, so using `class` is just less logical.

    Example:
  
    ```cpp
    template <typename T> ...
    ```

    instead of
  
    ```cpp
    template <class T> ...
    ```

8. Avoid type or usage-specific prefixes for variable, class or struct names. In particular, any type of Hungarian notation is disallowed.

    Two possible exceptions:
    - Constants may be prefixed with `k`, followed by an uppercase letter (following camel case convention).
    - Global non-constant variables with static linkage may be prefixed with `g`, followed by an uppercase letter (following camel case convention). However, avoid global state if possible (see below).

    Example:
  
    ```cpp
    class Widget; // OK
    class clWidget; // disallowed
    class IWidget; // disallowed
    bool bSuccess; // disallowed
    bool b_success; // disallowed
  
    namespace
    {
        constexpr std::size_t k_bits_per_element = 16; // OK
        std::atomic<int> g_instance_count; // OK
    }
    ```

9. For the naming of member variables, refer to the Classes section below.
    
---

[<<< Table of contents](../README.md)