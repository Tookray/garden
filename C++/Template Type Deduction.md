For

```cpp
template <typename T>
void f(ParamType param);

f(expr);
```

we need to consider three cases:

- `ParamType` is a pointer or reference (not universal `&&`).
	1. If `expr`'s type is a reference, ignore the reference part
	2. Then pattern-match `expr`'s type against `ParamType` to determine `T`

So

```cpp
#include <iostream>

template <typename T>
void f(T& t) {
	std::cout << __PRETTY_FUNCTION__ << std::endl;
}

template <typename T>
void g(T const& t) {
	std::cout << __PRETTY_FUNCTION__ << std::endl;
}

template <typename T>
void h(T *t) {
	std::cout << __PRETTY_FUNCTION__ << std::endl;
}

int main() {
	int x;
	int const y = x;
	int const &z = x;

	f(x); // void f(T&) [with T = int]
	f(y); // void f(T&) [with T = const int]
	f(z); // void f(T&) [with T = const int]

	g(x); // void g(const T&) [with T = int]
	g(y); // void g(const T&) [with T = int]
	g(z); // void g(const T&) [with T = int]

	h(&x); // void h(T*) [with T = int]
	h(&y); // void h(T*) [with T = const int]
}
```

This is why passing a `const` object to a template taking a `T&` or `T*` parameter is safe.

- `ParamType` is a universal reference.
	1. If `expr` is an l-value, both `T` and `ParamType` are deduced to be l-value references.
		- Notice that:
			- This is the only situation in template type deduction where `T` is deduced to be a reference.
			- Although `ParamType` is declared using the syntax for an r-value reference, its deduced type is an l-value reference.
	2. If `expr` is an r-value, the "normal" (case 1) rules apply.

So

```cpp
#include <iostream>

template <typename T>
void f(T&& t) {
	std::cout << __PRETTY_FUNCTION__ << std::endl;
}

int main() {
	int x;
	int const y = x;
	int const &z = x;

	f(x); // void f(T&&) [with T = int&]
	f(y); // void f(T&&) [with T = const int&]
	f(z); // void f(T&&) [with T = const int&]

	f(1); // void f(T&&) [with T = int]
}
```

- `ParamType` neither a pointer nor a reference (pass-by-value).
	1. If `expr`'s type is a reference, ignore the reference part.
	2. If, after ignoring `expr`'s reference-ness, `expr` is `const`, ignore that too. If it's `volatile`, also ignore that.

So

```cpp
#include <iostream>

template <typename T>
void f(T t) {
	std::cout << __PRETTY_FUNCTION__ << std::endl;

	t = 42;
}

int main() {
	int x;
	int const y = x;
	int const &z = x;

	f(x); // void f(T) [with T = int]
	f(y); // void f(T) [with T = int]
	f(z); // void f(T) [with T = int]

	 std::cout << x << " " << y << " " << z << std::endl; // 0 0 0
}
```

However, it gets even more complicated for array and function arguments.

>[!todo]
>Fill out this section.
