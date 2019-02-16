[<<< Table of contents](../README.md)

## Global state

1. Avoid keeping global state in translation units. Global state is strongly discouraged, unless needed for caching or synchronization purposes, in which case access should be thread-safe.
Even if you think that introducing global state might be a good idea, think twice before doing so. Often, more careful design can obviate the need for keeping global state.

    If a function or a class member function needs to access some state, either pass it as a (`const`) reference to the function, or keep it as class member variables.

    - <small>Global state is notoriously hard to keep track of. It complicates program logic in almost all cases, and also effectively prevents any kind of multithreading (unless potentially inefficient locks are introduced for each state access).</small>

    - <small>Furthermore, the order of initialization of static variables is implementation dependent. Any reliance on such an order will lead to hard-to-find bugs.</small>
    
---

[<<< Table of contents](../README.md)