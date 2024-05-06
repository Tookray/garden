## Source

- [tookray/hibiscus](https://github.com/tookray/hibiscus)

## Memory layout

The pages allocated from `mmap` are used to store two types of information:

1. **Block** - for internal usage.
2. **Data** - for external (user) usage.

![[hibiscus-memory-layout.excalidraw]]
