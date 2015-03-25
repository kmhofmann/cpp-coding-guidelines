# C++ Coding Conventions and Guidelines
-----------------------
Maintainer: Michael Hofmann (kmhofmann at gmail.com)

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

## Table of contents

* [0. Introduction](#section_introduction)
* [1. The big picture](#section_big_picture)
* [2. Code formatting](#section_code_formatting)
* [3. Comments](#section_comments)
* [4. Naming](#section_naming)
* [5. Control structures](#section_control_structures)
* [6. Classes](#section_classes)
* [7. Functions](#section_functions)
* [8. Variables](#section_variables)
* [9. Constants/Literals](#section_constants)
* [10. Enumerations](#section_enumerations)
* [11. Error handling](#section_error_handling)
* [12. Macros](#section_macros)
* [13. Namespaces](#section_namespaces)
* [14. Memory management](#section_memory_mgmt)
* [15. Casts](#section_casts)
* [16. Containers/Algorithms](#section_containers_algorithms)
* [17. Global state](#section_global_state)
* [18. File organization / Includes](#section_file_organization)
* [19. Miscellaneous](#section_misc)
* [20. Disallowed functions](#section_disallowed)
* [21. Compiler warnings](#section_warnings)
* [22. External libraries](#section_external_libraries)

## <a name="section_introduction"></a>0. Introduction

- This document provides a style guide for C++ code.
It provides both formatting conventions as well as guidelines for how to write good code, and what not to do.

  The document originates from a set of in-house C++ coding guidelines while the author was at [Blippar](http://www.blippar.com), but has and will continue to evolve beyond this use case.

- The guide assumes a certain level of familiarity with modern C++ features. While the rationale for a certain rule is often given, the guide is not meant to be an introductory document. Unlike other coding guidelines, the intention of the guidelines is *not* to limit usage of modern C++ features in any way.

- Sticking to a particular formatting convention ensures that code is written in a consistent fashion throughout the entire code base. The formatting convention described in this document is optimized for readability and for writing clean code.

  ![](http://imgs.xkcd.com/comics/code_quality.png "Code quality")


## <a name="section_big_picture"></a>1. The big picture

1. The language standard to be used is the current ISO standard, C++14. More modern language or library features should always be preferred over older, obsolete or less safe features or paradigms.

2. All features of the C++14 standard that are supported by the respective latest versions of the compilers used can and should be used whenever appropriate and useful.

  In case any code needs to be compiled by a not fully standards-conforming compiler (e.g. Visual C++ 2013 or 2015), refrain from using the unsupported features in the respective sections of the codebase.
  Modernize the codebase as soon as a new compiler version is released.

3. Refactor code whenever deemed necessary. Do not let technical debt accumulate, since this will lead to unmaintainable code very quickly.
Beautify and simplify code whenever and wherever possible, since this will increase readability and maintainability.

4. Think in abstractions and focus on genericity and reusability. If an algorithm or a data structure can be expressed generically without loss of performance, do not prematurely commit to only encoding a specific case. Alternatively, start with the specific case and genericize in the following step.
Avoid explicit duplication of algorithms or data structures; compilers are much better at doing this job (e.g. via template instantiation).

  - <small>In particular, consider putting any piece of code that is potentially reusable into an appropriate library. When doing so, ensure that your code fulfills the comparatively higher standards of library code, for example w.r.t genericity.</small>

5. Write correct and readable code first, micro-optimize later where necessary. Prefer elegant code over supposedly “hand-optimized” code that becomes obfuscated in the process. Optimize hot-spots in your code after judicious profiling.
On the other hand, do avoid premature pessimization. All other things, in particular readability, being equal, favor optimal code over pessimized alternatives.

  - <small>“Premature optimization is the root of all evil.” (Donald Knuth)</small>

  - <small>In many cases, programmers will severely underestimate the capabilities of the compiler optimizer, and therefore commit to inelegant, semi-obfuscated code a priori. This style of programming might have been acceptable in the ‘80s, but is not anymore today, including when targeting low-powered devices. Sometimes rigorous optimization is necessary after all, but in these cases profiling will prove the need. Know what your compiler optimizer can and cannot do.</small>

## <a name="section_code_formatting"></a>2. Code formatting

1. The general rule is: one statement, one line.
  
  However, break lines when they become too long. The notion of “too long” is somewhat flexible, depending on the line content. Usually, an `if` statement with several “or-ed” options, or function call arguments can be broken into several lines for enhanced readability. Similarly, function calls with lots of parameters can be broken over multiple lines to increase readability.
  
  We are explicitly not imposing a strict limit on the numbers of characters per line.

  - <small>Usually, about 120-140 characters on one line are fine, sometimes more, although the average line length should be considerably shorter.</small>

  - <small>Format for logical consistency first; hence the “one statement, one line” principle. Readability is not compromised by occasionally going over the above mentioned soft line limit. We are beyond the age of 80x24 terminal screens, so allowing a line limit beyond 80 characters is the right thing to do. A line limit of only 80 characters would likely impair readability, since all non-trivial statements would have to be broken across several lines. It also encourages a visually cramped coding style, which we very much discourage.</small>

2. This style guide does not intend to impose a definitive answer to the tabs vs. spaces question. Projects can follow either formatting convention, as long as all related projects are consistently written using the same style. The following two items provide suggestions for a sensible formatting style.

3. Consider using tab characters for *logical* indentation, and spaces for additional *visual* indentation:

  Use a mixture of tabs and spaces to indent broken lines; in this case tabs are to be used up to the current logical indentation level, followed by spaces for any further indentation (e.g. to line up statements). Do not use spaces for logical indentation, nor tabs for purely visual additional spacing.
  
  Example:

  ```
    {
    →   if ((x > 0 && x < width) ||     // pretend this is too long
    →   ····(y > 0 && y < height))
    }
```
  - <small>Using tabs, navigation through code is more efficient in most editors (e.g. pressing backspace or ‘cursor left’ once instead of multiple times).</small>

  - <small>Also, tabs provide a *logical* means for indentation instead of just a visual one, as opposed to spaces. Python even adopted tabs for enforced code structuring.</small>
  
    - <small>There is no problem with different tab spacing sizes for display purposes (see next item), as long as the above rule (logical vs. visual indentation) is followed consistently.</small>

4. The ideal tab spacing is 4 characters. Feel free to use an editor that displays a different tab width. Make sure to configure the editor in such a way that it does not replace tabs with spaces, if you have chosen to use tabs in your projects.

  - <small>4 space characters (per tab) provide a very good compromise. Using 2 characters looks visually cramped and leads to reduced readability (“which indentation level is this now?”), and more than 4 characters unnecessarily inflates line lengths. 3 is just odd -- pun intended.</small>

5. Opening braces (for classes, functions, control blocks, etc.) should *always* reside on a new empty line.

  ```
    for (int i = 0; i < 4; ++i)
    {
        // ...
    }
```

  Egyptian-style braces (where the opening brace is kept on the line end) are strongly discouraged.

  ```
    for (int i = 0; i < 4; ++i) {    // not allowed
        // ...
    }
    
    for (int i = 0; i < 4; ++i){     // not allowed
        // ...
    }
```

  - <small>This results in greatly enhanced code readability. It’s not just much easier to visually match braces when they are horizontally lined up, it is also more logical and consistent to keep both braces on the same indentation level. Control structure statements such as loops and if-statements are more cleanly separated from their body. We can also easily comment out the control statement if required, which would not be possible using Egyptian-style braces.</small>

  - <small>Some people like to refer to Kernighan and Ritchie or Stroustrup on their use of Egyptian-style braces in their respective C and C++ books, but obviously very different rules apply for code printed inside a book or in presentation slides, where it makes much more sense to save a bit of vertical space.

      Given the amount of lines that fit on modern monitors today, we do not need to save every bit of vertical whitespace at the great expense of code readability (and brace “matchability”).</small>

6. Whitespaces are to be inserted after keywords, but not after opening or before closing braces.

  All binary operators should have exactly one space on each side, except for `.`, `->`, `.*`, and `->*` which should have zero.

  Example:

  ```
    if (i == 5)    // not: if ( i == 5 ), or if (i==5) or if ( i==5), etc.
```

  ```
    for (const auto& widget : widgets)
```

  ```
    a = b + c;     // not: a=b+c; which impairs readability
    d = 2*a + 10;  // discouraged, but may be fine to emphasize precedence in complex formulas
```

  Sometimes, grouping mathematical operators and parentheses differently w.r.t whitespace might enhance readability. It is then okay to break this rule for innermost expressions.
  
  Example:
  
  ```
    a = ((b+c) + (d+e)) * f;
    ```

7. Cleanly separate the type from the variable name: Pointer (`*`) and reference (`&`) type designators are to be placed next to the type name without any whitespace. Variable names should be then placed after a whitespace.

  Avoid placing pointer and reference type symbols adjacent to a variable or function name.

  Example:
  
  ```
    int* p;
    int& q = *p;
```

  Avoid all of the following:
  
  ```
    int *p;
    int * p;
    int &q = *p;
    int & q = *p;
```

  - <small>Being a pointer is inherently part of the type of a variable, and not of the name. In the above example, the type is `int*` and the name is `p`. The type of a variable ("int”, or “int pointer”) belongs together, and has to be clearly separated from the name of the variable.</small>

8. Only ever declare one variable per line. Avoid placing multiple variable declarations on the same line.

  Example:
  
  ```  
    int x = 0;
    int y = 0;
```

  Not:

  ```
    int x = 0, y = 0;
```

  - <small>This greatly enhances code readability.</small>

  - <small>It also fixes the unfortunate language defect inherited from C, where `int* p, q;` declares one pointer and one non-pointer variable. Obviously, writing `int* p, *q;` or even `int *p, *q;` should be avoided, as it would break the rule above.</small>

9. Prefer not to arbitrarily align variable declaration/initialization groups.

  Example:
  
  ```
    int number = 5;
    std::string text = “Hello”;
```

  Not:

  ```
    int         number = 5;
    std::string text   = “Hello”;
```

  - <small>These kinds of alignments are just arbitrary. Instead, our formatting rule (e.g. “1 whitespace before and after the equals sign”) provides consistency.</small>

  - <small>In some cases, however, alignment can make sense, e.g. when defining the entries of a matrix.</small>

10. Consider separating

    - template specifiers,
    - the (optional) inline keyword,
    - their return type, and
    - the function name/arguments

  in function declarations and definitions by breaking them into separate lines, whenever the full declaration or definition becomes too long for quick visual inspection of its elements.

  Example: Consider writing

  ```
    template <typename T>
    inline
    T
    max(T a, T b)
    {
        // ...
    }
```

  instead of

  ```
    template <typename T> inline T max(T a, T b)
    {
        // ...
    }
```

  - <small>This makes it very easy to see the return type as well as the name of the function or member function. This might seem trivial in the above example (and should probably not be done in this case), but as template specifiers as well as return types can get unwieldy, it’s a good idea to visually separate them. Consider a declaration like:</small>

     ```
    template <typename BidirIt, typename IndexType>
    inline
    std::iterator_traits<BidirIt>::value_type
    Foo<ScalarType>::bar(BidirIt begin, BidirIt end, IndexType index);
```

      <small>and it becomes clear why a visual separation is important.</small>

11. Format function declarations and definitions consistently. There are a few possible styles for doing so.
The first one applies when the function name including arguments is short enough to fit on one line. In this case, keep all arguments on one line, as follows:

  ```
    void bar(double a0, double a1, double a2);
```

  If the function arguments do not fit on one line, either list each function argument on its own line, indented by two tabs (or the equivalent number of spaces). In this case, do not specify the first argument on the same line as the function name:
  
  ```
    void Foo<ElementType>::transmogrifyWidgetsAndGadgets(
    →   →   const std::vector<Widget>& widgets,
    →   →   const std::vector<Gadgets>& gadgets,
    →   →   double transmogrificationFactor = 1.0);
```

  Alternatively, line up all function arguments with whitespaces after the function name:
  
  ```
    void Foo<Many, Tmplt, Pars>::functionName(const std::vector<Widget>& widgets,
                                              const std::vector<Gadgets>& gadgets,
                                              double transmogrificationFactor = 1.0);
```

  The third style quickly becomes problematic in combination with long functions names, in particular when the class qualifier includes a number of template parameters. Readability might be decreased due to the excessive line length/indent. It is also much more tedious to reformat whenever the function name changes. Hence, the first two styles are preferred.

12. Make sure the formatting style of your function declaration always matches that of the definition.

## <a name="section_comments"></a>3. Comments

1. Aim to comment your code properly.

  Write both higher-level comments, as well as more detailed lower-level comments wherever appropriate. Any technically skilled colleague should be able to understand what is going on from both code as well as comments. Start-up costs related to code understanding should be kept to a minimum by commenting judiciously.

2. On the other hand, do not bury your code in comments. Omit any obvious comments (`++i; // increments i`). Well-written code should be largely self-documenting through choice of good variable and function names, logical program flows, etc.

3. All comments should be written in C++-style (`//`), with a variant of the C-style (`/*`) being mainly reserved for Doxygen-style documentation (`/**`).

  Always follow with a whitespace after starting a comment.
  
  Example:

  ```
    // This is a proper comment.
    //Don’t omit the whitespace.
```
  ```
    /* Don’t write cumbersome C-style comments either. */
```
  ```
    /**
     * @brief It’s ok and even encouraged for Doxygen-style documentation.
     */
```

   ```
    /// This is also Doxygen-style documentation.
```

  - <small>C-style comments can not be nested; C++-style comments can be (per line), and many IDEs support automatic commenting or uncommenting of code blocks. </small>

4. When providing Doxygen-style documentation, place it directly above the definition of a function/member function, not the initial declaration either inside or outside of a class.

  - <small>Doxygen-style documentation is meant to be read in a browser, not inside the code. Any reader of your class should be able to get a quick overview by looking at the public interface in-code, and the more of the class fits onto a screen, the easier it is to read. Do not blow up the size of your class definitions by providing in-line documentation.
Also, this is one more reason to cleanly separate function declaration from definition (see also below, in ‘Classes’ section).</small>

## <a name="section_naming"></a>4. Naming

1. There are generally two acceptable and internally consistent naming conventions.

  a) The "camel case" convention:
  ```
  class AwesomeWidget;              // classes: upper-case & camel case
  double distanceFromPoint = 5.0;   // variables: lower-case & camel case
  double computeDistance();         // functions: lower-case & camel case
  double AwesomeWidget::getFoo();   // member functions: lower-case & camel case
  enum class State { CorruptData }; // enums: upper-case & camel case 
```
  Only class names start with an upper-case letter, whereas variable and function names start with a lower-case letter. All following syllables are directly connected to the preceding syllable and begin with an upper-case letter.

  b) The "underscores" convention:
  ```
  class Awesome_Widget;              // classes: upper-case & underscore separators
  double distance_from_point = 5.0;  // variables: lower-case & underscore separators
  double compute_distance();         // functions: lower-case & underscore separators
  double Awesome_Widget::get_foo();  // member functions: lower-case & underscore separators
  enum class State { Corrupt_Data }; // enums: upper-case & underscore separators
```
  Only class names start with an upper-case letter, whereas variable and function names start with a lower-case letter. All following syllables are separated by an underscore (`_`) and keep the case of the preceding syllable(s).

  - <small>Camel case may be preferable to use of underscores, since the latter are considerably harder to type (quickly) several times in a word. There is no discernible difference in readability given names of reasonable length.</small>

2. Use one of these naming conventions consistently throughout all related projects. Do not mix styles or deviate from the chosen convention.

3. Class names and enumerations shall always begin with an upper-case letter, in order to cleanly distinguish them from (instance) variables. Never declare classes that begin with a lower-case letter.

  Example:
  
  ```
    class Widget;
    Widget widget;
    
    enum class Level : unsigned char;
```

  - <small>Class and enumeration types are not variable names, and a visual separation is clearly desirable. Using uppercase identifiers for the former and lowercase identifiers for the latter is an unambiguous way to distinguish the two, and everyone immediately knows which one is which.</small>

  - <small>Constructions like</small>

     ```
    widget my_widget; // funny name
    widget w; // name not expressive at all
```

     <small>will just lead to decreased readability at some point, and</small>
     
     ```
    widget widget; // the obvious naming in many cases
```

     <small>is not legal code (unless the first letter of the class type becomes uppercase).</small>

  - <small>There shall be no `class` or `struct` name in the codebase that begins with a lower-case letter.</small>

4. Variable names shall therefore always begin with a lower-case letter.

  Example:
  
  ```
    int index;
    Widget widget;
```
  - <small>There shall be no variable in the codebase that begins with an upper-case letter.</small>

5. Both member and non-member functions shall always begin with a lower-case letter.

  Example:

  ```
    double computeDistance();
    const std::string& Window::name();
```

  Not:
  
  ```
    double ComputeDistance();
```

  - <small>Function names are not class names, so prevent any possible confusion.</small>

  - <small>There shall be no function or member function in the codebase that begins with an upper-case letter.</small>

6. Any custom type names, e.g. in `typedef` or `using` statements, or in template declarations or definitions shall always begin with an uppercase letter.

  Example:
  
  ```
    using Scalar = float;
    template <typename BorderPolicy> class Interpolator;
```

7. Use expressive, descriptive and meaningful names for variables and function arguments. Avoid names like `p`, `a`, `b`, `c` etc. and arbitrary abbreviations like `cntr`.

  One-letter variable names may be used in for-loops if they’re used as simple counters, but the use of a more meaningful name is explicitly encouraged.
  
  Example:
  
  ```
    double f;       // this does
    std::size_t sz; // not help
    int i;          // readability
```

  ```
    double factor;    // much
    std::size_t size; // better
    int featureIndex; // :-)
```

  - <small>Good code should be self-documenting, not deliberately obfuscated. While there is a place for brevity (e.g. counter variables of some (but not all) for-loops), one should always err on the side of longer and more meaningful names. Remember, people besides yourself will have to read and/or work on your code, and be able to understand things quickly.</small>

8. For class template or function template declarations and definitions, use the `typename` keyword instead of the `class` keyword, since the former more accurately specifies what it is -- a type name. In the example below, `T` does not need to refer to an actual class, so using `class` is just less logical.

  Example:
  
  ```
    template <typename T> ...
```

  instead of
  
  ```
    template <class T> ...
```

9. Avoid type or usage-specific prefixes for variable, class or struct names. In particular, any type of Hungarian notation is disallowed.

  Two possible exceptions:
  - Constants may be prefixed with `k`, followed by an uppercase letter (following camel case convention).
  - Global non-constant variables with static linkage may be prefixed with `g`, followed by an uppercase letter (following camel case convention). However, avoid global state if possible (see below).

  Example:
  
  ```
  class Widget; // OK
  class clWidget; // disallowed
  class IWidget; // disallowed
  bool bSuccess; // disallowed
  
  namespace
  {
      constexpr std::size_t kBitsPerElement = 16; // OK
      std::atomic<int> gInstanceCount; // OK
  }
```

10. For the naming of member variables, refer to the Classes section below.

## <a name="section_control_structures"></a>5. Control structures

1. Each control structure block (`if` statements, `for`/`while`/`do-while` loops, `switch` blocks) should be separated with one empty line from code both above and below it, unless the code is an opening or closing brace from a higher scope. Do not put any code directly above or below a control structure block.

  Example:
  
  ```  
    {
        if (someFlag)
        {
            doSomething();
        }

        someCode();

        if (var == 5)
        {
            doSomething();
        }
        else if (var == 10)
        {
            doSomethingElse();
        }
        else
        {
            doSomethingElse();
        }

        someCode();

        for (const auto& widget : widgets)
        {
            // ...
        }

        someCode();
    }
```

  This is disallowed:

  ```
    int index = 0;
    for (const auto& widget : widgets)
    {
        // ...
    }
    functionCall();
```

  - <small>This greatly enhances code readability by visually separating the `if/else` blocks, `for-loop` blocks, etc. from surrounding code.</small>

2. Single-line if statements are only ever allowed with surrounding braces.

  ```
    if (someFlag)
    {
        doSomething();
    }
```

  This kind of code is not allowed at all:

  ```
    if (someFlag)
        doSomething();
```

  - <small> The former style provides greatly increased readability. Brace-related programming errors (or errors creeping in through code merges) are avoided. It’s too easy to “just add another line” without adding braces, and then days later follow up by debugging some weird error (that was caused by this error in judgement) for hours. Also, remember <a href="http://www.dwheeler.com/essays/apple-goto-fail.html">`goto fail; goto fail;`</a>?</small>

3. Any kind of assignments inside `if` statements are not allowed. Always split the assignment to a variable and a check against the variable over two lines.

  Example:

  ```
    if (f = someFunc())    // not allowed
    {
        // ...
    }
```

  Instead, write:
  
  ```
    f = someFunc();
   
    if (f)
    {
        // ...
    }
```

  - <small>Don’t try to be too clever. An assignment is not what one would usually expect inside an `if` statement.</small>

4. In `if` statements, Yoda conditions (i.e. writing `(<constant> == <variable>)`) are explicitly not allowed. We write the human readable `if (var == 2)` instead of the much less readable `if (2 == var)`.

  - <small>Instead of making code unreadable by introducing funny-looking equality comparisons, a much better way to protect oneself against accidental assignments (`if (var = 2)`) is to increase the warning level, and to compile cleanly at the highest warning level. All decent compilers will then emit a warning whenever an assignment inside an `if` statement is made. Make sure this warning is enabled.</small>

5. `case` statements that contain several statements inside or contain any local variables should be wrapped with braces. Cases that contain just one statement may be written on one line with the `case` label (and `break` statement, unless the case only consists of a `return` statement).

  The `break` statement should be placed inside the brace scope of each `case` label, so that the only statements outside of the respective brace scopes are the `case` labels themselves.

  Example:

  ```
    switch (someValue)
    {
        case 0: return 123; // this is a single-line case
    
        case 1:
        {
            int a = b; // this is a multi-line case
            b = c;
            break;
        }

        case 2:
            doSomething(); // this style is not allowed; use braces
            break;

        case 3: int a = b + 1; return a; // also not allowed

        default: break; 
    }
```

6. Avoid using fall-through cases (i.e. no `break` or `return` at the end of a `case` statement). Always provide either a `break` or a `return` statement at the end of each `case` statement. Code lacking either of these statements will be considered erroneous.

7. Add `default: break;` to the end of each `switch` statement that doesn’t consider all possible cases. This will avoid a compiler warning.

## <a name="section_classes"></a>6. Classes

1. Classes and structs are to be formatted and laid out as follows:

  ```
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

        double internalComputation();
    };

    Point::Point(double x, double y)
    : x_(x), y_(y)             // initializer list on separate line
    {
    }

    inline                       // inline keyword is at definition
    double
    Point::distance(const Point& other)
    {
        // ...
    }
```

  Stick to this ordering of elements (not all elements have to be present in every class):

    1. Constructors, in order of increasing complexity (e.g., default constructor first);
    2. Destructor;
    3. Copy & move constructors and assignment operators;
    4. Public member variables (**only if POD**);
    5. Public member functions;
    6. Protected member variables (**avoid, if possible**);
    7. Protected member functions;
    8. Private member variables;
    9. Private member functions.

  The initializer list for constructor definitions is always to be placed on a separate line.

2. Strongly prefer using classes (with all members `private`) over structs (with all members public), for the sake of a clean interface. This also allows invariants and other checks to be enforced in getter/setter functions, which greatly contributes to safer code.

3. Declare all data members `private`, if possible. Do not design classes with part of their member variables `private` and others `public`. This is usually a sign of lazy programming.

4. Place only functions into a `protected` section, never any member variables. In case you need to access the data of members in a derived class, do it via `protected` getter/setter functions in the base class.

5. Only use structs with `public` data members for plain data storage (of all members in the respective `struct`). Never make variables `public` when there is even the slightest risk that the user can modify these variables in such a way that the internal state becomes inconsistent.

  Example:
  (Do not do this; instead write an interface that cannot be used incorrectly.)
  
  ```
    struct GrayscaleImage
    {
        unsigned char* data; // anyone can let this point to
        std::size_t size;    // another memory location or change
                             // the size to an inconsistent value

        // ...
    };
```

  - <small>Rationale [for the three points above]: Declaring data members `private` allows syntactically uniform access to data, fine-grained access control, and provides implementation flexibility (i.e. the internals can change, but the interface stays constant).
See also Scott Meyers, “Effective C++”, 3rd Edition, Item 22.</small>

6. The order of access modifiers should always be `public`, then `protected`, then `private`. 

  - <small>The `public` interface should come first, so that everyone can quickly see how to use a class. Do not “hide” the `public` interface by putting the `private` or `protected` sections first. Consistency is crucial to ensure readability.</small>

  - <small>Avoid any repetition of access modifiers; e.g. a `private` section, followed by a `public` section, followed by a `private` section.</small>

7. Ensure that all non-POD classes and structs have proper constructors. Structs where a user has to explicitly assign the `struct` members are considered bad design.

  Bad example:
  
  ```
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

8. In constructor definitions, aim to initialize all variables via the initializer list. Explicit assignment inside the constructor is only allowed in cases where initialization via the initializer list is impossible.

  Example:
  ```
  Foo::Foo(int var0, double var1, Widget var2)
  : var0_(var0), var1_(var1), var2_(var2)
  {
      // var2_ = var2;  *avoid*
  }
```

  - <small>Explicit assignment inside the constructor leads to default construction of the member followed by re-assignment, which is often more inefficient than direct construction.</small>

9. Class member variables have to be initialized in constructor initializer lists in exactly the same order as they are declared in the class. This avoids potentially wrong initializations (and compiler warnings about it).

10. The initializer list can be formatted to either be on one single line, or contain each variable on a separate line. The latter case is preferable when many variables are to be initialized. Keep the comma at the end of the line and indent with two spaces at the beginning of each extra line.

  Example:
  
  ```
    Point::Point(float px, float py, float pz)
    : x(px), y(py), z(pz)

    Point::Point(float px, float py, float pz)
    : x(px),
      y(py),
      z(pz)
```

11. Always make sure that *all* member variables of a class are properly initialized in every constructor.

  Non-trivial types with default constructors often do not need to be listed explicitly should they be default constructed, but never leave built-in types such as integers or floating point numbers uninitialized.

12. Prefer to explicitly list the copy constructor, the copy assignment operator, the move constructor as well as the move assignment operator, even if they are just defaulted or deleted (as in the example above).

  - <small>The above mentioned constructors/operators can be default generated (without having to be mentioned in code) as long as the class does not have a user-defined destructor. However, for reasons of code clarity, it is greatly preferable to explicitly state the intent (e.g. defaulted, deleted, or custom implementation).
See also Scott Meyers, “Effective Modern C++”, Item 17.</small>

13. `private` or `protected` member variables are to be suffixed with an underscore (`_`).
`public` member variables should have no such suffix.
The use of prefixes such as `m_` for member variables is discouraged.
Never use a prefix underscore for any variable definition.

  - <small>The suffix underscore is quick and easy to type, and many C++ experts are using this style. It's a unique visual identifier for a (private or protected) member variable.</small>
  - <small>Prefix constructs like `m_index` look too much like Hungarian notation, which is discouraged. Besides, it’s harder to type and looks uglier.</small>
  - <small>Prefix underscores are not allowed by the standard in many cases (e.g. `__`, or `_` followed by an uppercase letter), so it’s better to not be tempted to use them.</small>

14. Cleanly separate member function declaration (inside a class) from member function definition (outside the class, and in a separate translation unit, if not a class template).
Exceptions can be made for trivial getter/setter functions, or for very small class implementations.

  Example:

  ```
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

  ```
    class Foo
    {
        int bar(int x)
        {
            // ... (non-trivial implementation)
        }
    };
```

  - <small>The class definition provides the interface that a “user” of the class can refer to and obtain a quick summary from. For this reason, it should not consist of a lot of implementation.</small>

15. Methods in translation units should be defined in *exactly* the same order as declared in header files. This greatly simplifies code readability and navigation.

16. Since `public` member variables (inside structs) have no trailing underscore (`_`), write constructors like this (i.e. use trailing underscores for the constructor function arguments instead):

  ```
    struct Point
    {
        double x;
        double y;

        Point(double x_, double y_)
        : x(x_), y(y_)
        { }
    };
```

17. Do not shadow variables declared in classes/structs. For example, do not name a local variable `x` if you have a public member variable `x` in the same class.

  Avoid constructions such as `this->x = x;`. Exception: when you need a nondependent name from a dependent base class (for a full explanation, <a href="https://isocpp.org/wiki/faq/templates#nondependent-name-lookup-members">see here</a>).

18. All virtual function overrides should be qualified with the `override` keyword.

  This enables better compile-time checking whether a function is indeed meant to be overridden, and can prevent the classical bug of either the function name or the arguments being all so slightly different.
  
  Virtual function overrides not qualified with the `override` keyword will be considered erroneous.

19. Always enforce const-correctness: Make functions that do not modify (visible) state `const`, otherwise non-const. The virality of const-ness is a feature, not a bug.

  Code that is not const-correct will be considered erroneous.

  - <small>`const` signifies *logical* const-ness. In cases where only internal state is modified (e.g. locking a mutex), make the state-changing variable `mutable`. Ensure all functions marked `const` work correctly in the face of multithreading and being accessed from multiple threads.</small>

## <a name="section_functions"></a>7. Functions

1. By default, return output parameters by value. Strongly prefer return-by-value over "returning" output parameters via non-const function arguments.

  If multiple types need to be returned, make use of `std::pair<>` or `std::tuple<>`, or return a custom struct/class if the number of return types would otherwise get too unwieldy.

  Example:
  
  ```
std::vector<int> getIndices(int seed);
std::tuple<int, float, double> computeNumbers();
CustomStructure getALotOfStuff();
```

  - <small>Output arguments should in almost all cases be returned by value, since this is where they belong. Given both move semantics as well as return value optimization (RVO) that all compilers implement, there are no inefficiencies associated with returning by value.</small>

2. By default, pass all input parameters with non-fundamental types by const reference (`const T&`). Pass all input parameters with fundamental types (e.g. `unsigned char`, `int`, `double`, etc.) by value (`T`).
  Only ever pass by const pointer (`const T* const`) when the input argument is truly optional; in this case, it should default to `nullptr`.

  Example:
  ```
Status writeImage(const std::string& filename, const Image& im, int compressionLevel);
```

  ```
  void processData(const Data& data, const OptionalData* const optionalData = nullptr);
```

3. If you really must use output parameters (for efficiency purposes in certain cases, e.g. to avoid memory allocations), pass them by non-const reference (`T&`).
  Only ever pass by non-const pointer (`T*`) when the output argument is optional; in this case, it should default to nullptr.
  
  Example:
  ```
void copy(const Image& src, const Rect& region, Image& dst);
void process(const Image& src, DebugInfo* debugInfo = nullptr);
```

4. It is strongly advisable and good practice in many of the "by-reference output parameter" cases to provide another convenience overload that returns said output parameter by value, unless the return value is used otherwise.

  Example:
  
  ```
    void detectEdges(const Image& img, Image& edgeImg)
    {
        // ...
    }
        
    Image detectEdges(const Image& img)
    {
        Image edgeImg;
        detectEdges(img, edgeImg);
        return edgeImg;
    }
```

5. Passing a variable by non-const reference when it can and should be passed as a const reference, or passing a non-optional variable by pointer when a reference should be used, is careless at least and will lead to mis-use and bugs at worst. Avoid this at all cost.

6. The `inline` keyword is an implementation detail and not part of the public interface of a class, so it is to be placed at the point of *definition* of a function, and *not* at its point of declaration. This equally applies to member functions.

  Example:
  
  ```
    class Foo
    {
        Foo(); // no inline keyword allowed here
    };

    inline Foo::Foo() // if used, inline keyword must be placed here
    {...}
```

## <a name="section_variables"></a>8. Variables

1. Always declare variables as locally as possible. Do not declare or define variables before they need to be declared/defined. C89-style variable declarations, where all variables are declared at the beginning of a function, are not allowed. 
In particular, define for-loop variables inside the for-loop if they are not used outside of the loop body.

  - <small>Sometimes, hoisting a variable outside of a scope can provide a valuable optimization.</small>

2. Always initialize variables at the point of definition, unless there is a very compelling (performance) reason not to so do.

3. Make use of automatic type deduction (via the `auto` keyword) wherever it makes sense. Aim to prefer using `auto` over explicit type declarations, but feel free to explicitly specify the type whenever it helps readability.
  
  Specify the type when explicit conversions are desired. In this case, consider using the left-to-right auto syntax (`auto var = Type(init);`) over the old-style syntax (`Type var(init);`). The former is more uniform, considering function return syntax and the `std::make_*` helper functions.
  
  These examples illustrate the consistency argument:
  
  ```
    auto intValue = 42;
    auto fpValue = 42.0f;
    auto e = Employee(id);
    auto widget = getWidget();
    auto w = std::make_unique<Widget>(1, 2, 3);
```

  We still consider the old-style syntax more readable for the definition of fundamental and some custom types, so this should be preferred:
  
  ```
  int valueI = 42;
  float valueF = 42.0f;
  Employee e(id);
```

  Use `auto` if you would otherwise have to repeat a type:
  
  ```
  auto w = std::make_unique<Widget>(1, 2, 3);
  // instead of:
  Widget w = std::make_unique<Widget>(1, 2, 3);
```

  Also, prefer using `auto` in for-loops (range based or otherwise), to not have to specify elaborate type names:
  
  ```
  for (auto itr = vec.begin(), itrEnd = vec.begin() + n; itr != itrEnd; ++itr) ...
  for (const auto& p : myMap)
  ...
```

  - <small>There is simply no need to explicitly specify a type in most cases; the compiler knows the type, often better than the programmer. Most importantly, automatic type deduction guarantees that no implicit conversions take place. This guarantee is not given with explicit type declaration, where the type might be wrong.</small>

     <small>Judicious use of auto makes code more readable, because complex type declarations can be omitted. It makes code also more maintainable, since type changes are automatically tracked and do not have to be done manually throughout the source code.</small>
     
  - <small>Do not use `auto` in some very specific cases, e.g. with proxy types or expression template libraries. For more details, refer to Scott Meyers, "Effective Modern C++", Item 6.</small>


## <a name="section_constants"></a>9. Constants/Literals

1. Constants are to be defined as `constexpr` or `const` variables, not as preprocessor defines. Using `#define` to define constants is strongly discouraged.

  - <small>The preprocessor is just a stupid text replacement tool and should be avoided at all cost. It is completely unaware of type safety/type checking and not obeying scoping. “Defined” constants are not visible in the debugger, their address cannot be taken, they cannot be passed by const-reference if needed, and they do create new “keywords” when put into header files.</small>

     <small>C++ provides vastly better tools for any imaginable use of `#define` outside of header include guards or conditional compilation.</small>

2. Consider putting integer constants into either an `enum class` (not an old-style `enum`; see below) or a (potentially type-specialized) class if they logically belong together.

3. Avoid global constants (outside of namespaces) by all means, as it leads to name conflicts. Instead place static `const[expr]` definitions into classes, or use anonymous namespaces inside translation units.

  Inside translation units, do not place constants outside of (anonymous) namespaces, since then other translation units could still access these variables via the `extern` keyword. (Needless to say, avoid any use of the `extern` keyword in this context.)

4. Numeric literals are to be specified by their precise type; implicit conversions by the compiler are not acceptable (unless unavoidable).

  The canonical form for floating point numbers has to be human-readable, i.e. omitted digits after the decimal point while providing the decimal point are not allowed.
  
  Example:
  
  ```
    float x = 1.0f;
    double x = 1.0;
    long x = 1l;
    unsigned long x = 1ul;
```
  
  Do not write:

  ```
    float x = 1.0;        // should be 1.0f
    float x = 1.f;        // should be 1.0f
    double x = 1.0f;      // should be 1.0
    double x = 1.;        // should be 1.0
    long x = 1;           // should be 1l
    unsigned long x = 1l; // should be 1ul
```

5. To specify numeric literals of a certain type, preferably use the literal suffix. Otherwise use the initializer form. Do not use C-style casts.

  Example:
  
  ```
    5l
    42ul
    unsigned char(16)
    short(100)
```
    
  Do not write:
  
  ```
    (long)5
    (unsigned char)16
    (short)100
```

## <a name="section_enumerations"></a>10. Enumerations

1. Enumeration types, as well as the contained enumerators, shall always begin with an uppercase letter. Avoid writing the enumerators in all-uppercase notation.

  Example:
  
  ```
    enum class Tide
    {
        Low,
        High
    };
```

  Not:
  
  ```
    enum class Tide
    {
        LOW,
        HIGH
    };
```

2. Use scoped enums (`enum class`) instead of old-style enums (`enum`), since enum classes do not pollute the surrounding scope.
Convert existing code using `enum` to make use of `enum class` instead.

  - <small>Scoped enums are not visible from the surrounding scope, greatly reducing the possibility of name clashes.</small>

  - <small>Scoped enums do not implicitly convert to `int`, whereas conventional enums do (which is often unwanted). Scoped enums can still be explicitly cast to an integer type.</small>

  - <small>The underlying type of a scoped enum can be explicitly specified. This also enables forward declaration of scoped enums.</small>

3. Consider providing an explicit type specification for an `enum class`, in order to guarantee the size of the enumeration, and to enable forward declaration. By default, the underlying type is `int`.

  Example:
  
  ```
    enum class Difficulty : unsigned char
    {
        Easy,
        Medium,
        Hard
    }
```

## <a name="section_error_handling"></a>11. Error handling

1. Use exceptions to signal abnormal failure of a function, i.e. the function is not able to perform its assigned task due to exceptional circumstances.

  Exceptional circumstances include but are not limited to: not being able to acquire a resource; failure of a constructor to establish the class invariant; out-of-range errors.
  
  Example:
  
  ```
    File::File(const std::string& filename) // has member: FILE* f_;
    {
        f_ = std::fopen(filename.c_str());

        if (f_ == nullptr)
        {
            throw std::runtime_error("File(): failed to open " + filename);
        }
}
```
  
2. Prefer using error return codes when the error state is a sometimes expected outcome of the function call; i.e. do not throw exceptions in conditions that can occur during normal program execution. Exceptions should be purely reserved for abnormal error handling.

  Example: Do not write

  ```
    void addWidget(const Widget& w)
    {
        // add widgets to queue; queue is limited to 10 elements
        if (queue_.size() >= 10)
        {
            throw QueueAdditionException("queue full");
        }
        
        queue_.push_back(w);
    }
```

  but rather write
  
  ```
    WidgetAdditionStatus addWidget(const Widget& w)
    {
        if (queue_.size() >= 10)
        {
            return WidgetAdditionStatus::QueueFull;
        }
        
        queue_.push_back(w);
        return WidgetAdditionStatus::Success;
    }
```    

3. Do not use `throw` as an alternative way to return a value from a function.

  Example: Do not write the following type of code, where the `throw` statement is blatantly misused as a possible return path.
  
  ```
    std::size_t findElement(const std::vector<int>& sortedData, int element)
    {
        auto itr = std::lower_bound(sortedData.cbegin(), sortedData.cend(), element);
        
        if (itr == sortedData.cend() || *itr != element)
        {
            throw std::runtime_error("element not found");
        }
        
        return std::distance(sortedData.cbegin(), itr);
    }
```


4. Use assertions to establish runtime invariants.

  Use assertions instead of exception handling to guarantee that invariants, e.g. pre-conditions or post-conditions, are being met. Exceptions should not be used for guarding against logic errors in your code.
  
  Example: Write
  
  ```
    int index = computeValidIndex(data);
    assert(index >= 0 && index <= data.size()); // Post-condition check
```

  instead of
  
  ```
    int index = computeValidIndex(data);
  
    if (index < 0 || index > data.size())
    {
        throw std::runtime_error("invalid index");
    }
```

5. Use `static_assert` to establish compile-time invariants.

  Use static assertions to guarantee that invariants known at compile time are being met. Since these are being checked at compile time and incur no runtime penalty, use them liberally.
  
  Example:
  
  ```
    template <typename T, typename U>
    T power(T base, U exponent)
    {
        static_assert(std::is_integral<U>::value, "exponent must be integer");
        // ...
    }
```  

6. Do not throw when being the direct owner of a resource; use RAII to prevent resource leaks instead.

  Example: Do not write
  
  ```
  void doThingsWithWidgets(int i)
  {
      Widget* widgets = new Widget[10];
      int i = func(widgets); // if this throws -> MEMORY LEAK
            
      if (i < 0)
      {
          throw std::runtime_error("bad value"); // MEMORY LEAK!
      }
      // ...
      delete widgets;
  }
```

  The following code uses RAII to guard against resource leaks:

  ```
  void doThingsWithWidgets(int i)
  {
      auto widgets = std::make_unique<Widget[10]>();
      int i = func(widgets); // may safely throw
            
      if (i < 0)
      {
          throw std::runtime_error("bad value");
      }
  }
```  
  
  However, avoid pointer semantics unless necessary. Use a local resource instead:

  ```
  void doThingsWithWidgets(int i)
  {
      std::vector<Widget> widgets(10);
      int i = func(widgets);
            
      if (i < 0)
      {
          throw std::runtime_error("bad value");
      }
  }
```

7. Do not overuse `try`/`catch` blocks.

  Explicit `try`/`catch` is verbose and leads to non-trivial and error prone code paths. Use RAII to minimize the use of `try`/`catch`. Also, do not try to catch every exception in every function; in particular do not re-throw exceptions. Instead, let an exception propagate until it can be handled in the correct place.

8. Make sure destructors, deallocation and `swap` can never fail.

  Destructors, resource deallocation and `swap` may never fail by throwing an exception. The standard library assumes that this is the case; if e.g. a destructor throws, standard library invariants are broken. Furthermore, if any destructor throws during stack unwinding as part of exception handling, `std::terminate()` will be called; it is not possible to recover from these cases in any way.
  
  Deallocation functions and `swap` must be explicitly `noexcept`.


## <a name="section_macros"></a>12. Macros

1. The use of macros is strongly discouraged. Avoid macros at all cost.

  - <small>The preprocessor is just a stupid text replacement tool and should be avoided at all cost. It is completely unaware of type safety/type checking and not obeying scoping. Defined macros are not debuggable and sooner or later will lead to nasty side effects (e.g. multiple increment problem).</small>

     <small>C\+\+ provides vastly better tools for any imaginable use of macros; use inline functions or lambdas instead. The use of `#define` outside of header include guards or conditional compilation has no place in modern C\+\+ programming.</small>

  - <small>Exception: A macro can still be useful (and the only way, unfortunately) to implement a custom assert() facility, when using the predefined macros __FILE__ and __LINE__.</small>

## <a name="section_namespaces"></a>13. Namespaces

1. Namespaces provide the proper means for code modularization and a means for logical grouping/decoupling of components. They specify programmer intent and help prevent polluting the global namespace.
  All library code should be part of a namespace, which is either a company-wide or library-wide identifier.

2. Use a fixed top-level namespace per library, and a sub-namespace for each sub-module inside the library.

  Example:
  All code for the library “foo” should be wrapped inside
  
  ```
    namespace foo
    {

        // ...

    } // namespace foo
```

  Make sure you avoid Egyptian-style braces (`namespace foo {`), as in every other case. Do not indent the code inside `namespace` statements.
  Always repeat the namespace statement as a comment after the closing brace (see example above), in order to allow quick visual matching of braces.

3. Use sub-namespaces whenever there is a logical separation between concerns. Always maintain this clear separation.

  Example:
  
  ```
    // file A:

    namespace foo
    {
    namespace img
    {
    
        // image processing stuff

    } // namespace img
    } // namespace foo
```
  ```
    // file B:
    
    namespace foo
    {
    namespace math
    {
    
        // maths related stuff

    } // namespace math
    } // namespace foo
```

4. The names of namespaces should be short, meaningful, concise, and all lower-case. Try to avoid use of underscores inside namespace names.

  Good namespace names are, e.g. `img`, `math`, `util`, etc.

  Bad namespace names are, e.g. `Utilities`, `txwz`, `my_nmspc`, etc.

  - <small>An exception to this rule can be a sub-namespace `namespace Private`, or `namespace Impl`, where the uppercase usage signals that all content is to be treated as implementation detail, not a public interface.</small>

5. Never ever use a `using namespace` statement at global or namespace scope in a header file.

  This is strongly disallowed, since it would pollute the global or respective namespace for every file that includes said header file.

6. Use of `using namespace` statements (except for `using namespace std;`!) is expressly permitted inside translation units, as long as the likelihood of ambiguity or naming clashes remains low, and readability remains preserved. If in doubt, explicitly prefix the namespace.

  Inside translation units, prefer using the `using namespace` statements at function scope instead of global scope.

7. Never write `using namespace std;`, even in translation units. All uses of standard library components should be explicitly prefixed by `std::` to make it clear that a standard component is being used, and to [prevent creation of name conflicts and ambiguities](https://isocpp.org/wiki/faq/coding-standards#using-namespace-std).

8. Do not use any kinds of prefixes to indicate membership of constants or functions to a library or module. This might have been a bad, but necessary "hack" in C code, but is considered bad style in C++ code. Namespaces provide a much better alternative.

## <a name="section_memory_mgmt"></a>14. Memory management

1. In general, aim to limit the use of pointers.

  Use value types or references (`const T&`, `T&`) for argument passing by default, and pointers only when not possible otherwise (e.g. for optional function arguments).

2. The use of pointer types as function arguments automatically indicates that the respective function argument is optional.
If a function argument is not optional, it should be passed by either const or non-const reference, for input and output arguments respectively.

3. By default, all members of a `class` or `struct` should be stack allocated, i.e. not be of any [smart] pointer type (`std::unique_ptr<>` or `std::shared_ptr`) unless justified.

4. The use of *purely observing* raw pointers is acceptable and can also be good practice, as long as lifetime guarantees of these observing pointers are respected.

   However, never ever use raw pointers as owning pointers outside of carefully hidden and wrapped-away low-level implementations (and only when absolutely necessary).

  For argument passing into functions, the above rules take precedence.

  - <small>Manual memory management with owning pointers is bug-prone and adds complexity which is often unnecessary. Pointers can also be null or point to uninitialized memory, whereas this is not possible when using references (thus eliminating a whole class of bugs).</small>

  - <small>Function arguments that are output variables should be taken as non-const references, unless the respective parameter is optional.</small>

5. Avoid manual memory management whenever possible; use the modern facilities instead. The use of `new`/`delete` or `malloc()`/`free()` (which ignores object construction) is strongly discouraged. Follow this "no naked new/delete" rule in all parts of the code.

  Use the RAII ("Resource Acquisition Is Initialization") idiom or smart pointers instead of `new`/`delete` or `malloc()`/`free()`, or make sure their use cases are cleanly wrapped away.

  - <small>Judicious use of RAII solves the problem of resource allocation and deallocation, and smart pointers (as particular instantiations of RAII) can replace almost all instances of manual memory management.</small>

  - <small>Never use unsafe APIs (e.g. APIs that expect a `new`-allocated object or pass back memory allocated inside a called function) directly, but always implement safe wrappers around them.</small>

6. `std::unique_ptr<>` and `std::shared_ptr<>` are the default facilities for all heap-based memory management (besides `std::vector<>` for arrays).

  Use `std::unique_ptr<>` for unique ownership of resources, and `std::shared_ptr<>` for shared ownership of resources. Only use `std::shared_ptr<>` when there is actual sharing of that resource, i.e. do not prematurely pessimize performance by requiring atomic reference counting when it is not actually needed.
  
  The use of `std::unique_ptr<>` incurs no performance penalty and is as efficient as "manual" memory allocation with `new` at even very moderate optimization levels.

7. Use only `std::make_unique<>` or `std::make_shared<>` to create the respective smart pointers, using the assignment operator:

  ```
    auto var = std::make_shared<Type>(args);
    auto var = std::make_unique<Type>(args);
```

  Strongly avoid using `new` inside a smart pointer constructor. The only cases where `new` inside a smart pointer constructor is acceptable are described in Scott Meyers, “Effective Modern C++”, Item 21 (mainly: custom deleters).
  
  Strongly avoid supplying an existing pointer, e.g. from a factory function, to the `std::unique_ptr<>` or `std::shared_ptr<>` constructors. This is usually bad and very brittle design; instead change the factory function itself to return a smart pointer. (See also above; no function should ever return an owning raw pointer.)

  ```
    std::shared_ptr<Type> var(new Type(args)); // strongly discouraged
    
    Type* rptr = pointerFactory(args);
    std::shared_ptr<Type> var(rptr); // even more strongly discouraged
```

  - <small>`std::make_unique` and `std::make_shared` comprise the accepted and modern idiom of writing smart pointer allocations. Using `new` inside a smart pointer constructor is technically not incorrect, but should be strictly avoided for reasons of consistency. Also, this would violate the strict pairing rule of `new` and `delete` (see next item).</small>

  - <small>Using `std::make_shared<>` is more efficient than heap allocation inside the `std::shared_ptr<>` constructor (avoiding one additional heap allocation for the reference count). Also, a construct such as `f(std::shared_ptr(new int(42)), g())` can leak memory, if `g()` throws an exception; this is not the case when using `std::make_shared<>`.</small>

8. If you really have to use `new` and `delete` (i.e. there is absolutely no other way, and their use is already cleanly wrapped), make sure they are always paired. There should be exactly one delete for every new, in the same source file.

9. Express the allocation of polymorphic objects as follows:

  ```
    auto pb = std::unique_ptr<Base>{std::make_unique<Derived>()};
```

  This old-style way also works

  ```
    std::unique_ptr<Base> pb = std::make_unique<Derived>();
```

  but the above left-to-right `auto` style is preferred, since it makes the pointer type conversion much more visible/explicit on the right hand side.

10. Prefer not to use raw arrays for stack-based array allocations. Aim to replace the use of arrays with the safer `std::array<>` class. There is no difference in performance at even very moderate optimization levels.

  Example:

  ```
    int values[5];
```
  should be replaced by:
  
  ```
    std::array<int, 5> values;
```

  - <small>Raw arrays don’t know their own size; `std::array<>` does. Compilers can offer range checks inside the std::array implementation, whereas this is not possible in all cases using raw array accesses. `std::array<>` offers (container-like) random access iterators whereas raw arrays do now, so well-written generic code is likely to just work, without (in some cases) having to be specialized. Also, `std::array<>` instances are assignable.</small>

11. Do not use `std::auto_ptr<>` at all. It has been fully replaced by the much more useful `std::unique_ptr<>` in C++11, officially deprecated in C++14, and completely removed from C++17.

## <a name="section_casts"></a>15. Casts

1. The best cast is a cast that is avoided.

2. If you have to cast, do not use any form of C style casts. Use `static_cast<>`, `dynamic_cast<>`, `const_cast<>` or `reinterpret_cast<>` instead, or a combination thereof.

  - <small>The names casts provide much better type checking as well as clarity on what’s the intent of the case. And we can easily search for the occurrence of a cast. Casts should be avoided if possible, so it is good if they look a bit ugly.</small>

3. For basic types, use the initializer form, if necessary: `int(a)`, instead of `(int)a`. 

  However, prefer initializing basic type variables with numbers using literals: `auto a = 10l;` or `unsigned long x = 42ul`.

4. Use of `dynamic_cast<>`, `const_cast<>` or `reinterpret_cast<>` should be avoided, unless there is no other alternative. Sometimes these are necessary; these occurrences should be safely hidden away inside an implementation. To elaborate:

  - `dynamic_cast<>` is easily overused, and frequently querying the type of an object at run-time (especially if in the form of a decision tree) points to a design problem. Consider virtual functions, potentially in combination with the Visitor pattern, as alternatives before resorting to RTTI.

  - Use of `const_cast<>` is strictly disallowed, unless interfacing with non-const correct legacy APIs that cannot be changed. Even then, breaking this contract between implementer and compiler should only be done after careful consideration.

  - `reinterpret_cast<>` is rightfully considered non-portable and should be mostly avoided. The cast can sometimes be useful if packed objects or arrays are to be viewed as a byte-stream, e.g. for serialization or strided access.

## <a name="section_containers_algorithms"></a>16. Containers/Algorithms

1. Make judicious use of the containers and algorithms of the standard library. The STL is your friend. Do not reinvent the wheel.

2. Prefer using `std::vector<>` over `std::list<>`, `std::deque<>`, or any of the heap-based associative containers such as `std::map<>` or `std::unordered_map<>` in a performance-sensitive context.

  The node-based structure of these containers make them fairly cache un-friendly, so performance considerations should be carefully balanced against their ease of use.
  `std::vector<>` on the other hand provides great cache locality, since its data storage is guaranteed to be contiguous.

3. Strongly prefer algorithms over hand-written loops. Use STL algorithms wherever appropriate.

  - <small> STL algorithms (headers `<algorithm>` and `<numeric>`) clearly express intent by their function name, and contain optimized implementations of their respective functionality. Using hand-written loops instead of an appropriate algorithm increases the risk of introducing bugs (due to reinventing the wheel, which is always bad).</small>

## <a name="section_global_state"></a>17. Global state

1. Avoid keeping global state in translation units. Global state is strongly discouraged, unless needed for caching or synchronization purposes, in which case access should be thread-safe.
Even if you think that introducing global state might be a good idea, think twice before doing so. Often, more careful design can obviate the need for keeping global state.

  If a function or a class member function needs to access some state, either pass it as a (`const`) reference to the function, or keep it as class member variables.

  - <small>Global state is notoriously hard to keep track of. It complicates program logic in almost all cases, and also effectively prevents any kind of multithreading (unless potentially inefficient locks are introduced for each state access).</small>

  - <small>Furthermore, the order of initialization of static variables is implementation dependent. Any reliance on such an order will lead to hard-to-find bugs.</small>

## <a name="section_file_organization"></a>18. File organization / Includes

1. Each header file should contain the definition of at most one `class` or `struct`, i.e. separate class definitions should reside in separate header files, with the possible exception of very small helper classes, structures or enums that are to be used only in context with the "main" class.

  Thus, there should be a header and a translation unit pair for each class, unless the implementation is header-only. The names of the header file and of the translation unit should correspond to the name of the main class.

  Example:
  
  Foo.hpp:
  
  ```
    class Foo
    {
        // ...
    };
```

  Foo.cpp:
  
  ```
    #include <path/to/Foo.hpp>

    Foo::Foo()
    {
        // ...
    }
```

  Alternatively, a header file/translation unit pair can be used to implement a collection of logically related functions.

  - <small>Rationale: Clear separation of interfaces. Make classes and their implementation easy to find.</small>

2. The file locations on disk should mirror the namespace organization.

  For example, inside a library called `mylib`, the `class mylib::img::Image<>` resides in a file `./mylib/src/mylib/img/Image.hpp`. The duplication of the library name is deliberate: the first occurrence in the path reflects the library directory, while the second occurrence reflects the namespace. These can, but do not necessarily have to match (e.g. the namespace might be an abbreviation).

3. Each library or application should require one and only one directory to be set as include directory for itself. Make sure all internal includes reflect this single root.

  In the example of 2., the include directory root would be the directory `./mylib/src`. Includes are then to be written as:
  
  ```
  #include <mylib/img/Image.hpp>
```

  making it completely clear which library and namespace `Image.hpp` belongs to.
  
  Strongly avoid adding additional include roots (such as `./mylib/src/mylib/img` in the above example), and, by extension, avoid writing shallow includes such as `#include "Image.hpp"`.

4. Include files and translation units are to be placed inside the same directory structure. There is no artificial separation between headers and implementation.

 - <small>Any separation between include files and translation units is bound to be arbitrary. Often, the include file contains the implementation, and there might not even be a corresponding translation unit. By keeping both files together, they are easily retrievable in your favorite IDE or file manager.</small>

  - <small>We are not writing redistributable libraries, where this separation might matter. Even then, include files that form a public API could be copied out in a post-build step.</small>

5. Translation unit files should have a `.cpp` extension, header files should end with `.hpp`. The use of `.h` file suffixes for C++ headers is discouraged.

  - <small>Rationale: Clearly disambiguate C\+\+ header files from C headers.</small>

6. Every file should either define include guards as follows:

  ```
    #ifndef <PROJ_NAME>_<NAMESPC1>_<NAMESPC2>_<...>_<FILENAME>_HPP
    #define <PROJ_NAME>_<NAMESPC1>_<NAMESPC2>_<...>_<FILENAME>_HPP

    // Your code

    #endif // <PROJ_NAME>_<NAMESPC1>_<NAMESPC2>_<...>_<FILENAME>_HPP
```

  or, alternatively, use the non-standard but very widely supported `#pragma once` directive:

  ```
    #pragma once        // This is very useful, indeed. But non-standard, so YMMV.
    
    // Your Code
```

  Do not mix include guards with `#pragma once` in the same library, i.e. all files of one library should make use of the same method.

  For include guards, the project name can be either the library name, or the name of the executable project. Do not omit this name. (Obvious exception: if the first namespace is equal to the project name, do not repeat this name.)

  - <small>Include guards must be unique. Imagine two projects with a top-level header called `Utils.hpp` and omitting the project name. In both cases, we would end up with `#ifndef UTILS_HPP`. Good luck finding the include conflict.</small>

7. In both header files as well as translation units, include *only* the headers that you are actually using. Use of “collector” headers (i.e. header files that include all headers of one library or module) is strongly discouraged, especially for header-only libraries.

  However, make sure the list of includes is complete and does not rely on transitive inclusions, i.e. implicitly assumed includes inside included headers.

  - <small>We don’t want to artificially increase compile times by including too much. We don’t want to include too little either - what might compile on one platform might not on the other, because include dependencies are handled differently inside the standard library.</small>

8. Prefer the order of includes happening in the following order:
  - standard library includes
  - external library includes (from most generic to most specific library)
  - internal library includes (from most generic to most specific library)
  - includes from current project

  Use empty lines liberally to delineate blocks of includes. Includes inside one block should ideally be ordered lexicographically, so that they can be easily found.
  
  Example:
  
  ```
    #include <array>
    #include <memory>
    #include <vector>

    #include <boost/filesystem.hpp>

    #include <Eigen/Core>

    #include <foo/Assert.hpp>
    #include <foo/Log.hpp>

    #include <bar/img/Convolution.hpp>
    #include <bar/img/Image.hpp>
```

9. Avoid using the `<xxx.h>` C compatibility headers; use the `<cxxx>` alternatives instead.

  The standard library headers in ISO C++ do not have any suffix. The standard headers from the C library, however, currently come in two versions. `<cxxx>` and `<xxx.h>`, where `xxx` is the basename of the header. Example: `<cstdio>` and `<stdio.h>`.
  
  Since the `<xxx.h>` C compatibility headers are a) not guaranteed to be available, b) deprecated and may be removed in a future version of the standard, and c) the implementation of the `<xxx.h>` headers may differ from the standard versions, [new projects should only use the `<cxxx>` versions of the C standard library headers](https://isocpp.org/wiki/faq/coding-standards#std-headers).

  - <small>The `<cxxx>` headers are only guaranteed to provide their declarations inside the `std` namespace. Some implementations choose to additionally provide the same declarations in the global namespace, but this is non-standard (and thus not guaranteed) behavior.</small>

## <a name="section_misc"></a>19. Miscellaneous

1. Avoid use of conditional compilation (`#ifdef`, or `#if defined()`), unless there is a very good reason for doing so (e.g. a NEON or SSE code path in-line).
If you have to use conditional compilation statements, avoid nesting them.

  Under absolutely no circumstances use conditional compilation in combination with multiple compilation of the same translation unit (a very poor template substitute in C).

  - <small>Conditional compilation can make code unreadable very quickly.</small>

2. Do not use multiple assignments within the same expression. Instead, place each assignment on a separate line in order to not decrease readability.

  Example:
  
  ```
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

## <a name="section_disallowed"></a>20. Disallowed functions

1. Strongly prefer C++ standard library facilities over C library functions, unless the latter absolutely cannot be avoided. In particular, aim to avoid all memory and string handling by C functions.

  Example:
  
  - `std::memcpy()` should be usually replaced by a call to `std::copy()`. The latter optimizes down to a `std::memcpy()` call in cases where POD types are to be copied, and is applicable in more situations (e.g. involving non-trivial copy constructors).
  
  - String handling should be done by using `std::string`. Avoid using `const char*` solutions and the use of any of the unsafe `str*` functions, unless there is a proven need to due performance considerations. This should be a very rare case.

2. Do not use the old C facilities (`rand()`, `RAND_MAX`) for random number generation, since they give no guarantees on the quality of the randomness of the generated values, which can be arbitrarily bad. Use `std::mt19937` (Mersenne Twister) from `<random>` by default, and other generators if you can justify their use over `std::mt19937`.

3. Do not use any of the following standard library functions, since they are officially removed from C++ 17 with the adoption of N4190:
`auto_ptr<>`, `random_shuffle()`, `ptr_fun()`, `mem_fun()`, `mem_fun_ref()`, `bind1st()`, `bind2nd()`, `unary_function`, `binary_function`.

## <a name="section_warnings"></a>21. Compiler warnings

1. All code should compile cleanly without any compiler warnings at the highest warning level. Disabling important warnings is not allowed, except selectively for third-party libraries.

  - <small>In almost all cases, if the compiler emits a warning, there is something wrong with the code. Write clean code, instead of ignoring or even disabling the warning.</small>

2. Use the `-Wall -Wextra` flags in GCC and Clang to enable most warnings, and enable more if deemed necessary.

  Consider using the `-Weverything` flag in Clang to enable *all* warnings (a flag absent from GCC), and then explicitly disable the ones deemed unimportant.
  
  Use the `/W4` flag in Visual C++.

## <a name="section_external_libraries"></a>22. External libraries

1. External libraries can offer useful functionality at relatively little implementation/integration cost. However, for every well-written library there are many shoddily written and/or architected libraries.
Make sure you pick the good ones and stay clear of the bad ones. Have a close look at the APIs that the library provides. Are they bullet-proof and type-safe? How well is the library maintained? How modern is its coding style?

2. Before using an external library for a certain task, make sure that the desired functionality is not already included in the C++ standard library.
In any case, use the standard library by default and as much as possible - it provides efficient container and algorithm implementations, and much more.

3. [Boost](http://www.boost.org/) provides many useful libraries; however, many of these have been superseded by the C++ standard library implementations since C++11. Use Boost in cases where the desired functionality is not available yet via the standard (e.g. Boost.Filesystem, or Boost.Geometry), and the standard library in all other cases.

  Note that using some of the Boost libraries may greatly increase compile times. Stay away from the complex metaprogramming facilities, unless you can prove a need. If you need to use them, make sure they are only included in (few) translation units, but not headers.

4. Use [Eigen](http://eigen.tuxfamily.org/), the de-facto standard library for general-purpose linear algebra, for all linear algebra tasks or matrix representations.
With Eigen at our disposal, it is not acceptable anymore to use raw arrays for vector/matrix operations, or to “roll our own” matrix classes.

  For very simple tasks that involve mostly 4x4 matrices, up to 4-element vectors or quaternions (e.g. rendering), consider using [GLM](http://glm.g-truc.net/) or [MathFu](https://google.github.io/mathfu/) library if you think Eigen is too heavy for the task (it usually isn't).

5. [OpenCV](http://opencv.org/) should only ever be considered for prototyping code. Even then it should be avoided or (if its use is necessary) cleanly wrapped away. Do not use OpenCV in production-level code.

  - <small>OpenCV contains lots of ancient, hardly readable, hardly documented code, written in various styles. New development seems to be focused on adding features quickly via "community" involvement, instead of cleaning up piles of technical debt. Large parts of the library should probably be completely rewritten, and backwards compatibility with ancient C client code should be given up. Many of the new additions are questionable and of varying quality.</small>

  - <small>Most of its interfaces are based on runtime decisions using type-unsafe switches (“if this int equals CV_XXX, then algorithm XXX is chosen”) — one of the worst ways of designing an interface. Going any deeper than the rather limited API-provided options will inevitably end in a world of pain, a.k.a reading the source code. Most of the OpenCV functionality is not designed to be easily extensible.</small>

  - <small>Its most basic data representations (`cv::Mat`, `cv::InputArray`, etc.) are based on constant disregard and/or coercion of the type system, which completely undermines the trust in any other part of the library. The library contains some very bad and careless design decisions, and the fact that it "seems to mostly work" is not a good excuse for using it.</small>

  - <small>The library adds lots of binary size bloat when integrated on any platform, even when just using a small number of their modules or when trying to custom-compile only the needed files.</small>

6. Strongly consider using C libraries only after wrapping them with a modern interface, especially with respect to modern memory management (RAII). (First, obviously, consider not using C libraries in the first place, but search for a modern and well-written C++ alternative.)

  C library wrappers might be one of the few good reasons to apply the Pimpl idiom, in order to constrain any includes of C headers (and their respective `#define` hell) to a well-firewalled translation unit. Make sure that such “unhygienic” C library headers are never used in own library/project headers.