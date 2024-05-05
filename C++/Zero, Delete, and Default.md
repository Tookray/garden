What is the difference between `= 0` and `= delete` (and `= default`)?

- `= 0` is used to denote that a function is a [[Pure Virtual Function]]

The other two are a bit more complicated and requires some context.

- `= delete`
- `= default`

The compiler implicitly generates constructors when none are defined.

```cpp
#include <iostream>

class A {
private:
	int a;

public:
	void print() const {
		std::cout << this->a << std::endl;
	}
};

int main() {
	A a;
	A b(a);
	A c = a;

	a.print();
	b.print();
	c.print();
}
```

Compiling and executing the above yields:

```console
$ g++ main.cpp -o bin && ./bin
0
0
0
```

So, the compiler must have generated the following:

1. Default construct
2. Copy constructor
3. Copy assignment operator

However, if we define a copy constructor.

```cpp
#include <iostream>

class A {
private:
	int a;

public:
	// Define a copy constructor...
	A(A const& other) {
		a = other.a;
	}

	void print() const {
		std::cout << this->a << std::endl;
	}
};

int main() {
	A a;
	A b(a);
	A c = a;

	a.print();
	b.print();
	c.print();
}
```

The code won't compile!

```console
$ g++ main.cpp -o bin && ./bin
main.cpp: In function ‘int main()’:
main.cpp:19:11: error: no matching function for call to ‘A::A()’
   19 |         A a;
      |           ^
main.cpp:9:9: note: candidate: ‘A::A(const A&)’
    9 |         A(A const& other) {
      |         ^
main.cpp:9:9: note:   candidate expects 1 argument, 0 provided
```

This means that <mark style="background: #FFB86CA6;">the compiler did not generate a default constructor since we provided a constructor of our own</mark>. However, we can tell the compiler to generate a default constructor using `= default`.

```cpp
#include <iostream>

class A {
private:
	int a;

public:
	A() = default;

	// Define a copy constructor!
	A(A const& other) {
		a = other.a;
	}

	void print() const {
		std::cout << this->a << std::endl;
	}
};

int main() {
	A a;
	A b(a);
	A c = a;

	a.print();
	b.print();
	c.print();
}
```

Now the code compiles and runs.

```console
$ g++ main.cpp -o bin && ./bin
0
0
0
```

`= delete` is used to mark functions as deleted which means that <mark style="background: #FFB86CA6;">any use of it results in a compilation error</mark>.

```cpp
class A {
public:
	A() = delete;
};

int main() {
	A a;
}
```

The above won't compile.

```console
$ g++ main.cpp -o bin && ./bin
main.cpp: In function ‘int main()’:
main.cpp:7:11: error: use of deleted function ‘A::A()’
    7 |         A a;
      |           ^
main.cpp:3:9: note: declared here
    3 |         A() = delete;
      |         ^
```

Traditionally, we used to make methods private...

```cpp
class A {
private:
	A() {} // You can't default construct A!
};
```

But `= delete` makes it inaccessible even from contexts that can see `private` like within the class and `friend`s, so there is no uncertainty.
