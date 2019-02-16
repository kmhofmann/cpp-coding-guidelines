[<<< Table of contents](../README.md)

## Error handling

1. Use exceptions to signal abnormal failure of a function, i.e. the function is not able to perform its assigned task due to exceptional circumstances.

    Exceptional circumstances include but are not limited to: not being able to acquire a resource; failure of a constructor to establish the class invariant; out-of-range errors.
  
    Example:
  
    ```cpp
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

    ```cpp
    void add_widget(const Widget& w)
    {
        // add widgets to queue; queue is limited to 10 elements
        if (queue_.size() >= 10)
        {
            throw Queue_addition_exception("queue full");
        }
        
        queue_.push_back(w);
    }
    ```

    but rather write
  
    ```cpp
    Widget_addition_status add_widget(const Widget& w)
    {
        if (queue_.size() >= 10)
        {
            return Widget_addition_status::Queue_full;
        }
        
        queue_.push_back(w);
        return Widget_addition_status::Success;
    }
    ```    

3. Do not use `throw` as an alternative way to return a value from a function.

    Example: Do not write the following type of code, where the `throw` statement is blatantly misused as a possible return path.
  
    ```cpp
    std::size_t find_element(const std::vector<int>& sorted_data, int element)
    {
        auto itr = std::lower_bound(sorted_data.cbegin(), sorted_data.cend(), element);
        
        if (itr == sorted_data.cend() || *itr != element)
        {
            throw std::runtime_error("element not found");
        }
        
        return std::distance(sorted_data.cbegin(), itr);
    }
    ```


4. Use assertions to establish runtime invariants.

    Use assertions instead of exception handling to guarantee that invariants, e.g. pre-conditions or post-conditions, are being met. Exceptions should not be used for guarding against logic errors in your code.
  
    Example: Write
  
    ```cpp
    int index = compute_valid_index(data);
    assert(index >= 0 && index <= data.size()); // Post-condition check
    ```

    instead of
  
    ```cpp
    int index = compute_valid_index(data);
  
    if (index < 0 || index > data.size())
    {
        throw std::runtime_error("invalid index");
    }
    ```

5. Use `static_assert` to establish compile-time invariants.

    Use static assertions to guarantee that invariants known at compile time are being met. Since these are being checked at compile time and incur no runtime penalty, use them liberally.
  
    Example:
  
    ```cpp
    template <typename T, typename U>
    T power(T base, U exponent)
    {
        static_assert(std::is_integral<U>::value, "exponent must be integer");
        // ...
    }
    ```  

6. Do not throw when being the direct owner of a resource; use RAII to prevent resource leaks instead.

    Example: Do not write
  
    ```cpp
    void do_things_with_widgets(int i)
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

    ```cpp
    void do_things_with_widgets(int i)
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

    ```cpp
    void do_things_with_widgets(int i)
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
    
---

[<<< Table of contents](../README.md)