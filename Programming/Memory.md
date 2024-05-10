## Memory alignment

The processor does not read and write memory in byte-sized chunks; instead, it accesses memory in groups of two-, four-, eight-byte chunks—it could be even larger like 16 or 32!

For example, if the processor reads two-bytes at a time and we want to read four-bytes starting at address 1, we'll need to read three chunks instead of two!

![[memory-alignment.excalidraw]]

The processor must perform an extra memory access which slows down the operation.

For a more in-depth overview, take a look at:

- https://developer.ibm.com/articles/pa-dalign/
