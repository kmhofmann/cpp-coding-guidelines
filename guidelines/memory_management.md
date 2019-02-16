[<<< Table of contents](../README.md)

## Memory management

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

    ```cpp
    auto var = std::make_shared<Type>(args);
    auto var = std::make_unique<Type>(args);
    ```

    Strongly avoid using `new` inside a smart pointer constructor. The only cases where `new` inside a smart pointer constructor is acceptable are described in Scott Meyers, “Effective Modern C++”, Item 21 (mainly: custom deleters).
  
    Strongly avoid supplying an existing pointer, e.g. from a factory function, to the `std::unique_ptr<>` or `std::shared_ptr<>` constructors. This is usually bad and very brittle design; instead change the factory function itself to return a smart pointer. (See also above; no function should ever return an owning raw pointer.)

    ```cpp
    std::shared_ptr<Type> var(new Type(args)); // strongly discouraged
    
    Type* rptr = pointer_factory(args);
    std::shared_ptr<Type> var(rptr); // even more strongly discouraged
    ```

    - <small>`std::make_unique` and `std::make_shared` comprise the accepted and modern idiom of writing smart pointer allocations. Using `new` inside a smart pointer constructor is technically not incorrect, but should be strictly avoided for reasons of consistency. Also, this would violate the strict pairing rule of `new` and `delete` (see next item).</small>

    - <small>Using `std::make_shared<>` is more efficient than heap allocation inside the `std::shared_ptr<>` constructor (avoiding one additional heap allocation for the reference count). Also, a construct such as `f(std::shared_ptr(new int(42)), g())` can leak memory, if `g()` throws an exception; this is not the case when using `std::make_shared<>`.</small>

8. If you really have to use `new` and `delete` (i.e. there is absolutely no other way, and their use is already cleanly wrapped), make sure they are always paired. There should be exactly one delete for every new, in the same source file.

9. Express the allocation of polymorphic objects as follows:

    ```cpp
    auto pb = std::unique_ptr<Base>{std::make_unique<Derived>()};
    ```

    This old-style way also works

    ```cpp
    std::unique_ptr<Base> pb = std::make_unique<Derived>();
    ```

    but the above left-to-right `auto` style is preferred, since it makes the pointer type conversion much more visible/explicit on the right hand side.

10. Prefer not to use raw arrays for stack-based array allocations. Aim to replace the use of arrays with the safer `std::array<>` class. There is no difference in performance at even very moderate optimization levels.

    Example:

    ```cpp
    int values[5];
    ```
    
    should be replaced by:
  
    ```cpp
    std::array<int, 5> values;
    ```

    - <small>Raw arrays don’t know their own size; `std::array<>` does. Compilers can offer range checks inside the std::array implementation, whereas this is not possible in all cases using raw array accesses. `std::array<>` offers (container-like) random access iterators whereas raw arrays do now, so well-written generic code is likely to just work, without (in some cases) having to be specialized. Also, `std::array<>` instances are assignable.</small>

11. Do not use `std::auto_ptr<>` at all. It has been fully replaced by the much more useful `std::unique_ptr<>` in C++11, officially deprecated in C++14, and completely removed from C++17.

    
---

[<<< Table of contents](../README.md)