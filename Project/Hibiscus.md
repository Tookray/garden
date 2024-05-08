## Source

- [tookray/hibiscus](https://github.com/tookray/hibiscus)

## Memory layout

The pages allocated from `mmap` are used to store two types of information:

1. **Block** - for internal usage.
2. **Data** - for external (user) usage.

![[hibiscus-memory-layout.excalidraw]]

>[!note]
>The pages don't really have to be exactly `4096` bytes each, we can ask `mmap` to give us larger regions of memory as well. I suppose an arena could be a couple pages large.

## Managing block pages

How should I manage the block pages? For example, let's say that I want to represent the blocks using a list or tree data structure, then that requires us to allocate memory for nodes. But what happens if the size of the node is variable?

It feels like a never-ending dependency! Where do I store the block information? Where do I store the block-manager information? Where do I store the block-manager-manager information? Argh!

I think implementation would be easier if we used fixed-sized memory for each page (or arena) since we'll know how many slabs we can fit. Because if we know that, then we know how many nodes we need, and thus also know how large the block page needs to be. But I also want to try implementing a buddy system... decisions, decisions.

I have an interesting idea for how to manage nodes. At the start of the linked list, I have all my in-use nodes, and at the end of the list I have all the free-to-use nodes. When you need a node, you take one from the end and insert it in your linked list! You could also have a separate list consisting of just free nodes. How obvious of a solution was this?!

The main idea is that <mark style="background: #FFB86CA6;">after allocating the page, we create all the nodes at the start</mark>, this way, we don't have to worry about (de)allocating the nodes as the program runs—at least, until we need to allocate another page.
