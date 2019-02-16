[<<< Table of contents](../README.md)

## Code formatting

1. Formatting is important, but developers tend to have strong opinions about it.
Consistency in a code base is probably more important than following every guideline in this section as it was written.

2. The general guideline is: one statement, one line.
  
    However, break lines when they become too long. The notion of “too long” is somewhat flexible, depending on the line content and personal taste.
    Usually, an `if` statement with several “or-ed” options, or function call arguments can be broken into several lines for enhanced readability.
    Similarly, function calls with lots of parameters can be broken over multiple lines to increase readability.
  
    This guide is not imposing a strict limit on the numbers of characters per line, but this can be set and enforced on a per-project level.

    - <small>Usually, about 120-140 characters on one line are fine, sometimes more, although the average line length should be considerably shorter.</small>

    - <small>Format for logical consistency first; hence the “one statement, one line” principle. Readability is not compromised by occasionally going over the above mentioned soft line limit. We are beyond the age of 80x24 terminal screens, so allowing a line limit beyond 80 characters is the right thing to do. A line limit of only 80 characters would likely impair readability, since all non-trivial statements would have to be broken across several lines. It also encourages a visually cramped coding style, which we very much discourage.</small>

3. This style guide does not intend to impose an answer to the tabs vs. spaces question. Projects can follow either formatting convention, as long as all related projects are consistently written using the same style. The following two items provide suggestions for a sensible formatting style.

4. Either use all spaces, without use of tabs. Alternatively, consider using tab characters for *logical* indentation, and spaces for additional *visual* indentation. In this case, tabs are to be used up to the current logical indentation level, followed by spaces for any further indentation (e.g. to line up statements).
  
    Example:

    ```cpp
    {
    ····if ((x > 0 && x < width) ||     // pretend this is too long
    ········(y > 0 && y < height))
    }
    ```
  
    or
    
    ```cpp
    {
    →   if ((x > 0 && x < width) ||     // pretend this is too long
    →   ····(y > 0 && y < height))
    }
    ```
    - <small>Using tabs, navigation through code is more efficient in most editors (e.g. pressing backspace or ‘cursor left’ once instead of multiple times). When using spaces, reformatting often takes more key strokes, even in editors supporting "tabbed" spaces well.</small>
  
    - <small>When following the above suggestion consistently, there is no problem with different tab spacing sizes for display purposes.</small>

5. Choose the indentation width as you like, but be consistent throughout your whole code base.

    - <small>Good and common choices include 2 or 4 spaces. The former may look visually cramped to some people.</small>
    
    - <small>Using an indentation width of >4 spaces is very uncommon.</small>

6. Opening braces (for classes, functions, control blocks, etc.) should reside on a new empty line.

    ```cpp
    for (int i = 0; i < 4; ++i)
    {
        // ...
    }
    ```

    Egyptian-style braces (where the opening brace is kept on the line end) are discouraged.

    ```cpp
    for (int i = 0; i < 4; ++i) {    // discouraged
        // ...
    }
    ```

    - <small>This results in greatly enhanced code readability. It’s not just much easier to visually match braces when they are horizontally lined up, it is also more logical and consistent to keep both braces on the same indentation level. Control structure statements such as loops and if-statements are more cleanly separated from their body. We can also easily comment out the control statement if required, which would not be possible using Egyptian-style braces.</small>

    - <small>Some people like to refer to Kernighan and Ritchie or Stroustrup on their use of Egyptian-style braces in their respective C and C++ books, but obviously very different rules apply for code printed inside a book or in presentation slides, where it makes much more sense to save a bit of vertical space.

      Given the amount of lines that fit on modern monitors today, we do not need to save every bit of vertical whitespace at the great expense of code readability (and brace “matchability”).</small>

7. Whitespaces are to be inserted after keywords, but not after opening or before closing braces.

    All binary operators should have exactly one space on each side, except for `.`, `->`, `.*`, and `->*` which should have zero.

    Example:

    ```cpp
    if (i == 5)    // not: if ( i == 5 ), or if (i==5) or if ( i==5), etc.
    ```

    ```cpp
    for (const auto& widget : widgets)
    ```

    ```cpp
    a = b + c;     // not: a=b+c; which impairs readability
    d = 2*a + 10;  // discouraged, but may be fine to emphasize precedence in complex formulas
    ```

    Sometimes, grouping mathematical operators and parentheses differently w.r.t whitespace might enhance readability. It is then okay to break this rule for innermost expressions.
  
    Example:
  
    ```cpp
    a = ((b+c) + (d+e)) * f;
    ```

8. Cleanly separate the type from the variable name: Pointer (`*`) and reference (`&`) type designators are to be placed next to the type name without any whitespace. Variable names should be then placed after a whitespace.

    Avoid placing pointer and reference type symbols adjacent to a variable or function name.

    Example:
  
    ```cpp
    int* p;
    int& q = *p;
    ```

    Avoid all of the following:
  
    ```cpp
    int *p;
    int * p;
    int &q = *p;
    int & q = *p;
    ```

    - <small>Being a pointer is inherently part of the type of a variable, and not of the name. In the above example, the type is `int*` and the name is `p`. The type of a variable ("int”, or “int pointer”) belongs together, and has to be clearly separated from the name of the variable.</small>

9. Prefer not to arbitrarily align variable declaration/initialization groups.

    Example:
  
    ```cpp
    int number = 5;
    std::string text = “Hello”;
    ```

    Not:

    ```cpp
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

    ```cpp
    template <typename T>
    inline
    T
    max(T a, T b)
    {
        // ...
    }
    ```

    instead of

    ```cpp
    template <typename T> inline T max(T a, T b)
    {
        // ...
    }
    ```

    - <small>This makes it very easy to see the return type as well as the name of the function or member function. This might seem trivial in the above example (and should probably not be done in this case), but as template specifiers as well as return types can get unwieldy, it’s a good idea to visually separate them. Consider a declaration like:</small>

       ```cpp
      template <typename BidirIt, typename IndexType>
      inline
      std::iterator_traits<BidirIt>::value_type
      Foo<ScalarType>::bar(BidirIt begin, BidirIt end, IndexType index);
      ```

      <small>and it becomes clear why a visual separation is important.</small>

11. Format function declarations and definitions consistently. There are a few possible styles for doing so.
The first one applies when the function name including arguments is short enough to fit on one line. In this case, keep all arguments on one line, as follows:

    ```cpp
    void bar(double a0, double a1, double a2);
    ```

    If the function arguments do not fit on one line, either list each function argument on its own line, indented by two tabs (or the equivalent number of spaces). In this case, do not specify the first argument on the same line as the function name:
  
    ```cpp
    void Foo<ElementType>::transmogrify_widgets_and_gadgets(  // no arguments on this line
            const std::vector<Widget>& widgets,
            const std::vector<Gadgets>& gadgets,
            double transmogrification_factor = 1.0);
    ```

    Alternatively, line up all function arguments with whitespaces after the function name:
  
    ```cpp
    void Foo<Many, Tmplt, Pars>::function_name(const std::vector<Widget>& widgets,
                                               const std::vector<Gadgets>& gadgets,
                                               double transmogrification_factor = 1.0);
    ```

    The third style quickly becomes problematic in combination with long functions names, in particular when the class qualifier includes a number of template parameters. Readability might be decreased due to the excessive line length/indent. It is also much more tedious to reformat whenever the function name changes. Hence, the first two styles are preferred.

12. Make sure the formatting style of your function declaration always matches that of the definition.
    
---

[<<< Table of contents](../README.md)