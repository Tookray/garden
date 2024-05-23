The <mark style="background: #FFB86CA6;">address space</mark> is an abstraction of the physical memory, and it is the running program's view of the memory in the system. The address space of a process contains all of the memory state (including code, stack, and heap).

The OS *virtualizes* memory, and the process works with *virtual* addresses which the OS (with the help of hardware) converts to *physical* addresses.

The goals of <mark style="background: #FFB86CA6;">virtual memory</mark> are:

- Transparency
- Efficiency
- Protection (and isolation)

![[13. Address Space.pdf]]
