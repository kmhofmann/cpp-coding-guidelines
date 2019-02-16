[<<< Table of contents](../README.md)

## Disallowed functions

1. Strongly prefer C++ standard library facilities over C library functions, unless the latter absolutely cannot be avoided. In particular, aim to avoid all memory and string handling by C functions.

    Example:
  
    - `std::memcpy()` should be usually replaced by a call to `std::copy()`. The latter optimizes down to a `std::memcpy()` call in cases where POD types are to be copied, and is applicable in more situations (e.g. involving non-trivial copy constructors).
  
    - String handling should be done by using `std::string`. Avoid using `const char*` solutions and the use of any of the unsafe `str*` functions, unless there is a proven need to due performance considerations. This should be a very rare case.

2. Do not use the old C facilities (`rand()`, `RAND_MAX`) for random number generation, since they give no guarantees on the quality of the randomness of the generated values, which can be arbitrarily bad. Use `std::mt19937` (Mersenne Twister) from `<random>` by default, and other generators if you can justify their use over `std::mt19937`.

3. Do not use any of the following standard library functions, since they are officially removed from C++ 17 with the adoption of N4190:
`auto_ptr<>`, `random_shuffle()`, `ptr_fun()`, `mem_fun()`, `mem_fun_ref()`, `bind1st()`, `bind2nd()`, `unary_function`, `binary_function`.
    
---

[<<< Table of contents](../README.md)