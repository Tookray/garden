An `enum` can be <mark style="background: #FFB86CA6;">scoped or unscoped</mark>.

```cpp
// Unscoped
enum Colour {
	Red,
	Green,
	Blue,
};

// Scoped
enum class Colour {
	Red,
	Green,
	Blue,
};
```

Unliked unscoped `enum`s, the scoped `enum`'s names are local to the `enum` and do not implicitly convert to other types like `int`.

>[!info] Tip
>Use `using enum <name>` (requires C++20) to introduce the names in an `enum` in to the current scope.
