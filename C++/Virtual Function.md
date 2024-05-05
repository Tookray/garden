When a virtual function is overridden, <mark style="background: #FFB86CA6;">the most-derived version is used at all levels of the class hierarchy</mark>.

```cpp
#include <iostream>

class A {
public:
	virtual void speak() const {
		std::cout << "A\n";
	}
};

class B : public A {
public:
	void speak() const override {
		std::cout << "B\n";
	}
};

void speak(A const& a) {
	a.speak();
}

int main() {
	A a;
	B b;

	speak(a); // A
	speak(b); // B
}
```
