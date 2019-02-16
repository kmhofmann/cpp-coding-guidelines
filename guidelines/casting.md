[<<< Table of contents](../README.md)

## 15. Casting

1. The best cast is a cast that is avoided.

2. If you have to cast, do not use any form of C style casts. Use `static_cast<>`, `dynamic_cast<>`, `const_cast<>` or `reinterpret_cast<>` instead, or a combination thereof.

    - <small>The names casts provide much better type checking as well as clarity on what’s the intent of the case. And we can easily search for the occurrence of a cast. Casts should be avoided if possible, so it is good if they look a bit ugly.</small>

3. For basic types, use the initializer form, if necessary: `int(a)`, instead of `(int)a`. 

    However, prefer initializing basic type variables with numbers using literals: `auto a = 10l;` or `unsigned long x = 42ul`.

4. Use of `dynamic_cast<>`, `const_cast<>` or `reinterpret_cast<>` should be avoided, unless there is no other alternative. Sometimes these are necessary; these occurrences should be safely hidden away inside an implementation. To elaborate:

    - `dynamic_cast<>` is easily overused, and frequently querying the type of an object at run-time (especially if in the form of a decision tree) points to a design problem. Consider virtual functions, potentially in combination with the Visitor pattern, as alternatives before resorting to RTTI.

    - Use of `const_cast<>` is strictly disallowed, unless interfacing with non-const correct legacy APIs that cannot be changed. Even then, breaking this contract between implementer and compiler should only be done after careful consideration.

    - `reinterpret_cast<>` is rightfully considered non-portable and should be mostly avoided. The cast can sometimes be useful if packed objects or arrays are to be viewed as a byte-stream, e.g. for serialization or strided access.