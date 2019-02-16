[<<< Table of contents](../README.md)

## Functions

1. By default, return output parameters by value. Strongly prefer return-by-value over "returning" output parameters via non-const function arguments.

    If multiple types need to be returned, make use of `std::pair<>` or `std::tuple<>`, or return a custom struct/class if the number of return types would otherwise get too unwieldy.

    Example:
  
    ```cpp
    std::vector<int> get_indices(int seed);
    std::tuple<int, float, double> compute_numbers();
    Custom_structure get_a_lot_of_stuff();
    ```

    - <small>Output arguments should in almost all cases be returned by value, since this is where they belong. Given both move semantics as well as return value optimization (RVO) that all compilers implement, there are no inefficiencies associated with returning by value.</small>

2. By default, pass all input parameters with non-fundamental types by const reference (`const T&`). Pass all input parameters with fundamental types (e.g. `unsigned char`, `int`, `double`, etc.) by value (`T`).
    Only ever pass by const pointer (`const T* const`) when the input argument is truly optional; in this case, it should default to `nullptr`.

    Example:
  
    ```cpp
    Status write_image(const std::string& filename, const Image& im, int compression_level);
    ```

    ```cpp
    void process_data(const Data& data, const Optional_data* const optional_data = nullptr);
    ```

3. If you really must use output parameters (for efficiency purposes in certain cases, e.g. to avoid memory allocations), pass them by non-const reference (`T&`).
  Only ever pass by non-const pointer (`T*`) when the output argument is optional; in this case, it should default to nullptr.
  
    Example:
  
    ```cpp
    void copy(const Image& src, const Rect& region, Image& dst);
    void process(const Image& src, Debug_info* debug_info = nullptr);
    ```

4. It is strongly advisable and good practice in many of the "by-reference output parameter" cases to provide another convenience overload that returns said output parameter by value, unless the return value is used otherwise.

    Example:
  
    ```cpp
    void detect_edges(const Image& img, Image& edge_img)
    {
        // ...
    }
        
    Image detect_edges(const Image& img)
    {
        Image edge_img;
        detect_edges(img, edge_img);
        return edge_img;
    }
    ```

5. Passing a variable by non-const reference when it can and should be passed as a const reference, or passing a non-optional variable by pointer when a reference should be used, is careless at least and will lead to mis-use and bugs at worst. Avoid this at all cost.

6. The `inline` keyword is an implementation detail and not part of the public interface of a class, so it is to be placed at the point of *definition* of a function, and *not* at its point of declaration. This equally applies to member functions.

    Example:
  
    ```cpp
    class Foo
    {
        Foo(); // no inline keyword allowed here
    };

    inline Foo::Foo() // if used, inline keyword must be placed here
    {...}
    ```
    
---

[<<< Table of contents](../README.md)