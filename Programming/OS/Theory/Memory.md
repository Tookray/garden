The <mark style="background: #FFB86CA6;">address space</mark> is an abstraction of the physical memory, and it is the running program's view of the memory in the system. The address space of a process contains all of the memory state (including code, stack, and heap).

The OS *virtualizes* memory, and the process works with *virtual* addresses which the OS (with the help of hardware) converts to *physical* addresses.

The goals of <mark style="background: #FFB86CA6;">virtual memory</mark> are:

- Transparency
- Efficiency
- Protection (and isolation)

![[13. Address Space.pdf]]

This section is really just about

- `malloc`
- `free`
- `calloc`
- `realloc`

and the associated dangers with misuse including:

- Forgetting to allocate memory (usually results in a <mark style="background: #FFB86CA6;">segmentation fault</mark>)
- Not allocating enough memory (<mark style="background: #FFB86CA6;">buffer overflow</mark>)
- Forgetting to initialize memory (<mark style="background: #FFB86CA6;">uninitialized read</mark>)
- Forgetting to free memory (<mark style="background: #FFB86CA6;">memory leak</mark>)
- Freeing memory before you are done with it (<mark style="background: #FFB86CA6;">dangling pointer</mark>)
- Freeing memory repeatedly (<mark style="background: #FFB86CA6;">double free</mark>, undefined behaviour)
- Calling `free()` incorrectly (<mark style="background: #FFB86CA6;">invalid free</mark>)

![[14. Memory API.pdf]]
