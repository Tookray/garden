What does it mean when there is a `const` keyword at the end of a function?

```cpp
class Foo {
public:
	int Bar(int baz) const {
	}
};
```

The `const` keyword changes the type of `this` from `Foo* const` to `const Foo* const`.

In other words, `Bar` is now a `const` function. <mark style="background: #FFB86CA6;">Changes to a data member of the class will now lead to a compilation error.</mark>
