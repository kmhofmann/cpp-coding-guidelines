[<<< Table of contents](../README.md)

## External libraries

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
    
---

[<<< Table of contents](../README.md)