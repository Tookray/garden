There are various types that us to perform <mark style="background: #FFB86CA6;">byte-oriented access to memory</mark>. These types include (but are not limited to):

- `char *`
- `size_t`
- `uintptr_t`
- `std::byte *`

But which one should we use? Well, it's complicated, and there is no real hard requirements, but it looks like the rule of thumb is that

- `char *`: sequences of textual characters.
- `void *`: type-erasure scenarios.
	- The pointed-to data is typed, but for some reason a typed pointer must not be used or it cannot be determined whether it's typed or not.
- `uintptr_t`: don't use this for pointer arithmetic.
	- Any arithmetic operations on it are integer arithmetic, not pointer arithmetic.
- `std::byte *`: for raw memory.

## Reference

- https://stackoverflow.com/questions/45435875/with-stdbyte-standardized-when-do-we-use-a-void-and-when-a-byte
