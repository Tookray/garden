A pure [[Virtual Function]] is denoted by the `virtual` keyword and `= 0` (note that this is not the same thing as `= delete`, see [[Zero, Delete, and Default]]).

```cpp
class A {
public:
	virtual void fn() = 0;
};
```

<mark style="background: #FFB86CA6;">A pure virtual function must be implemented by the derived class</mark> (unless the derived class itself is abstract). Also, <mark style="background: #FFB86CA6;">when a class has a virtual method, it cannot be instantiated</mark>.

Trying to compile:

```cpp
class A {
public:
	virtual void foo() = 0;
};

int main() {
	A a;
}
```

results in a compilation error:

```console
$ g++ main.cpp
main.cpp: In function ‘int main()’:
main.cpp:9:11: error: cannot declare variable ‘a’ to be of abstract type ‘A’
    9 |         A a;
      |           ^
main.cpp:3:7: note:   because the following virtual functions are pure within ‘A’:
    3 | class A {
      |       ^
main.cpp:5:22: note:     ‘virtual void A::foo()’
    5 |         virtual void foo() = 0;
      |                      ^~~
```
