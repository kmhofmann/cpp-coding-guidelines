[<<< Table of contents](../README.md)

## STL

1. Make judicious use of the containers and algorithms of the standard library. The STL is your friend. Do not reinvent the wheel.

2. Prefer using `std::vector<>` over `std::list<>`, `std::deque<>`, or any of the heap-based associative containers such as `std::map<>` or `std::unordered_map<>` in a performance-sensitive context.

    The node-based structure of these containers make them fairly cache un-friendly, so performance considerations should be carefully balanced against their ease of use.
    `std::vector<>` on the other hand provides great cache locality, since its data storage is guaranteed to be contiguous.

3. Strongly prefer algorithms over hand-written loops. Use STL algorithms wherever appropriate.

    - <small> STL algorithms (headers `<algorithm>` and `<numeric>`) clearly express intent by their function name, and contain optimized implementations of their respective functionality. Using hand-written loops instead of an appropriate algorithm increases the risk of introducing bugs (due to reinventing the wheel, which is always bad).</small>
    
---

[<<< Table of contents](../README.md)