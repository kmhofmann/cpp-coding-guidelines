[<<< Table of contents](../README.md)

## File organization / Includes

1. Each header file should ideally contain the definition of at most one `class` or `struct`, i.e. separate class definitions should reside in separate header files, with the possible exception of very small helper classes, structures or enums that are to be used only in context with the "main" class.

    Thus, there should be a header and a translation unit pair for each class, unless the implementation is header-only. The names of the header file and of the translation unit should correspond to the name of the main class.

    Example:
  
    Foo.hpp:
  
    ```cpp
    class Foo
    {
        // ...
    };
    ```

    Foo.cpp:
  
    ```cpp
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
  
    ```cpp
    #include <mylib/img/Image.hpp>
    ```

    making it completely clear which library and namespace `Image.hpp` belongs to.
  
    Strongly avoid adding additional include roots (such as `./mylib/src/mylib/img` in the above example), and, by extension, avoid writing shallow includes such as `#include "Image.hpp"`.

4. Include files and translation units are to be placed inside the same directory structure. There is no artificial separation between headers and implementation.

    - <small>Any separation between include files and translation units is bound to be arbitrary. Often, the include file contains the implementation, and there might not even be a corresponding translation unit. By keeping both files together, they are easily retrievable in your favorite IDE or file manager.</small>

5. Translation unit files should have a `.cpp` extension, header files should end with `.hpp`. The use of `.h` file suffixes for C++ headers is discouraged.

    - <small>Rationale: Clearly disambiguate C\+\+ header files from C headers.</small>

6. Every file should either define include guards as follows:

    ```cpp
    #ifndef <PROJ_NAME>_<NAMESPC1>_<NAMESPC2>_<...>_<FILENAME>_HPP
    #define <PROJ_NAME>_<NAMESPC1>_<NAMESPC2>_<...>_<FILENAME>_HPP

    // Your code

    #endif // <PROJ_NAME>_<NAMESPC1>_<NAMESPC2>_<...>_<FILENAME>_HPP
    ```

    or, alternatively, use the non-standard but very widely supported `#pragma once` directive:

    ```cpp
    #pragma once        // This is convenient but non-standard, so YMMV.
    
    // Your code
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
  
    ```cpp
    #include <array>
    #include <memory>
    #include <vector>

    #include <boost/filesystem.hpp>

    #include <Eigen/Core>

    #include <foo/Assert.hpp>
    #include <foo/Log.hpp>

    #include <bar/Widget.hpp>
    #include <bar/Gadget.hpp>
    ```

9. Avoid using the `<xxx.h>` C compatibility headers; use the `<cxxx>` alternatives instead.

    The standard library headers in ISO C++ do not have any suffix. The standard headers from the C library, however, currently come in two versions. `<cxxx>` and `<xxx.h>`, where `xxx` is the basename of the header. Example: `<cstdio>` and `<stdio.h>`.
  
    Since the `<xxx.h>` C compatibility headers are a) not guaranteed to be available, b) deprecated and may be removed in a future version of the standard, and c) the implementation of the `<xxx.h>` headers may differ from the standard versions, [new projects should only use the `<cxxx>` versions of the C standard library headers](https://isocpp.org/wiki/faq/coding-standards#std-headers).

    - <small>The `<cxxx>` headers are only guaranteed to provide their declarations inside the `std` namespace. Some implementations choose to additionally provide the same declarations in the global namespace, but this is non-standard (and thus not guaranteed) behavior.</small>
    
---

[<<< Table of contents](../README.md)