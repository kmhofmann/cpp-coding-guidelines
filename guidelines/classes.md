[<<< Table of contents](../README.md)

## Classes

1. Classes and structs should be formatted and laid out in a consistent order, such as:

    ```cpp
    class Point
    {
    public:
        Point();
        Point(double x, double y);

        Point(const Point&) = default;
        Point& operator=(const Point&) = default;
        Point(Point&&) = default;
        Point& operator=(Point&&) = default;

        double distance(const Point& other);

    private:
        double x_;    // private member variables come before
        double y_;    // any private member functions

        double internal_computation();
    };

    Point::Point(double x, double y)
        : x_(x), y_(y)           // initializer list on separate line
    {
    }

    inline                       // inline keyword is at definition
    double Point::distance(const Point& other)
    {
        // ...
    }
    ```

    Prefer this ordering of elements (not all elements have to be present in every class):

      1. Constructors, in order of increasing complexity (e.g., default constructor first);
      2. Destructor;
      3. Copy & move constructors and assignment operators;
      4. Public member variables (**only if POD**);
      5. Public member functions;
      6. Protected member variables (**avoid, if possible**);
      7. Protected member functions;
      8. Private member variables;
      9. Private member functions.

    The initializer list for constructor definitions should always be placed on a separate line, to enhance readability.

2. Strongly aim to declare all member variables `private`, to increase information hiding.
Try to prevent designing classes with part of their member variables `private` and others `public`. 
`private` member variables allow invariants and other checks to be enforced via getter/setter functions, which greatly contributes to safer code.

3. Place only functions into a `protected` section, never any member variables. In case you need to access the data of members in a derived class, do it via `protected` getter/setter functions in the base class.

4. Only use structs with `public` data members for plain data storage (of all members in the respective `struct`). Never make variables `public` when there is even the slightest risk that the user can modify these variables in such a way that the internal state becomes inconsistent.

    Example:
    (Do not do this; instead write an interface that cannot be used incorrectly.)
  
    ```cpp
    struct Grayscale_image
    {
        unsigned char* data; // anyone can let this point to
        std::size_t size;    // another memory location or change
                             // the size to an inconsistent value

        // ...
    };
    ```

    - <small>Rationale [for the three points above]: Declaring data members `private` allows syntactically uniform access to data, fine-grained access control, and provides implementation flexibility (i.e. the internals can change, but the interface stays constant).
See also Scott Meyers, “Effective C++”, 3rd Edition, Item 22.</small>

5. The order of access modifiers should be `public`, then `protected`, then `private`. 

    - <small>The `public` interface should come first, so that everyone can quickly see how to use a class. Do not “hide” the `public` interface by putting the `private` or `protected` sections first. Consistency is crucial to ensure readability.</small>

    - <small>Avoid any repetition of access modifiers; e.g. a `private` section, followed by a `public` section, followed by a `private` section.</small>

6. Ensure that all non-POD classes and structs have proper constructors. Structs where a user has to explicitly assign the `struct` members are considered bad design.

    Bad example:
  
    ```cpp
    struct Point // where is the constructor?
    {
        float x; // missing
        float y; // value
        float z; // initialization
    };

    Point p;    // Oops, uninitialized memory here!
    p.x = 0.0f; // Oops, forgot to assign to z. This should
    p.y = 0.0f; // not even be possible with proper class design.
    ```

7. In constructor definitions, aim to initialize all variables via the initializer list. Explicit assignment inside the constructor is only allowed in cases where initialization via the initializer list is impossible.

    Example:
  
    ```cpp
    Foo::Foo(int var0, double var1, Widget var2)
        : var0_(var0), var1_(var1), var2_(var2)
    {
        // var2_ = var2;  *avoid*
    }
    ```

    - <small>Explicit assignment inside the constructor leads to default construction of the member followed by re-assignment, which is often more inefficient than direct construction.</small>

8. Class member variables have to be initialized in constructor initializer lists in exactly the same order as they are declared in the class. This avoids potentially wrong initializations (and compiler warnings about it).

9. Always make sure that *all* member variables of a class are properly initialized in every constructor.

    Non-trivial types with default constructors often do not need to be listed explicitly should they be default constructed, but never leave built-in types such as integers or floating point numbers uninitialized.

10. Prefer to explicitly list the copy constructor, the copy assignment operator, the move constructor as well as the move assignment operator, even if they are just defaulted or deleted (as in the example above).

    - <small>The above mentioned constructors/operators can be default generated (without having to be mentioned in code) as long as the class does not have a user-defined destructor. However, for reasons of code clarity, it is greatly preferable to explicitly state the intent (e.g. defaulted, deleted, or custom implementation).
See also Scott Meyers, “Effective Modern C++”, Item 17.</small>

11. `private` or `protected` member variables should be suffixed with an underscore (`_`), to distinguish them from `public` member and non-member variables.
`public` member variables should have no such suffix.
Never use a prefix underscore for any variable definition.

    - <small>The suffix underscore is quick and easy to type, and many C++ experts are using this style. It's a unique visual identifier for a (private or protected) member variable.</small>
    - <small>Prefix constructs like `m_index` look too much like Hungarian notation, which is discouraged. Besides, it’s harder to type and looks uglier.</small>
    - <small>Prefix underscores are not allowed by the standard in many cases (e.g. `__`, or `_` followed by an uppercase letter), so it’s better to not be tempted to use them.</small>

12. Cleanly separate member function declaration (inside a class) from member function definition (outside the class, and in a separate translation unit, if not a class template).
Exceptions can be made for trivial getter/setter functions, or for very small class implementations.

    Example:

    ```cpp
    class Foo
    {
        int bar(int x);
    };

    int Foo::bar(int x)
    {
        // ... (non-trivial implementation)
    }
    ```

    Not:

    ```cpp
    class Foo
    {
        int bar(int x)
        {
            // ... (non-trivial implementation)
        }
    };
    ```

    - <small>The class definition provides the interface that a “user” of the class can refer to and obtain a quick summary from. For this reason, it should not consist of a lot of implementation.</small>

13. Methods in translation units should be defined in the same order as declared in header files. This greatly simplifies code readability and navigation.

14. Since `public` member variables (inside structs) have no trailing underscore (`_`), write constructors like this (i.e. use trailing underscores for the constructor function arguments instead):

    ```cpp
    struct Point
    {
        double x;
        double y;

        Point(double x_, double y_)
        : x(x_), y(y_)
        { }
    };
    ```

15. Do not shadow variables declared in classes/structs. For example, do not name a local variable `x` if you have a public member variable `x` in the same class.

    Avoid constructions such as `this->x = x;`. Exception: when you need a nondependent name from a dependent base class (for a full explanation, <a href="https://isocpp.org/wiki/faq/templates#nondependent-name-lookup-members">see here</a>).

16. All virtual function overrides should be qualified with the `override` keyword.

    This enables better compile-time checking whether a function is indeed meant to be overridden, and can prevent the classical bug of either the function name or the arguments being all so slightly different.
  
    Virtual function overrides not qualified with the `override` keyword will be considered erroneous.

17. Always enforce const-correctness: Make functions that do not modify (visible) state `const`, otherwise non-const. The virality of const-ness is a feature, not a bug.

    Code that is not const-correct will be considered erroneous.

    - <small>`const` signifies *logical* const-ness. In cases where only internal state is modified (e.g. locking a mutex), make the state-changing variable `mutable`. Ensure all functions marked `const` work correctly in the face of multithreading and being accessed from multiple threads.</small>
    
---

[<<< Table of contents](../README.md)