## Pool allocator

Consider the following memory layout:

![[roselle-pool-alignment.excalidraw]]

Is the size of the header padding (blue) and data padding (red) constant throughout the page? Since the header `struct` consists of a single pointer, it's size and alignment is `8` bytes. How about the data? Well, its aligned on either: `2`, `4`, `8`, `16`, and so on.

![[roselle-pool-alignment-example-1.excalidraw]]

From the example, it looks like the padding size if fixed throughout the page, however, I'm not sure how I would prove it. So let's just run a bunch of experiments... and it turns out that the padding is not necessarily always constant.

```
Header size:       8
Header alignment:  8
Data size:        48
Data alignment:   32

Header padding:  0, Data padding: 24
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
Header padding:  0, Data padding:  8
```

Hmm... let's try to visualize it.

![[roselle-pool-alignment-example-2.excalidraw]]

Yep, my observation is most definitely wrong.
