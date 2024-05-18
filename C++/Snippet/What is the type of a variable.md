This is a surprisingly difficult task, however, following the conversation over at Stack Overflow, we find that the following snippet is what we are looking for.

```cpp
#include <string_view>

namespace TypeName {

template <typename T>
constexpr std::string_view wrapped_type_name() {
#ifdef __clang__
	return __PRETTY_FUNCTION__;
#elif defined(__GNUC__)
	return  __PRETTY_FUNCTION__;
#elif defined(_MSC_VER)
	return  __FUNCSIG__;
#endif
}

class probe_type;

constexpr std::string_view probe_type_name("TypeName::probe_type");
constexpr std::string_view probe_type_name_elaborated("class TypeName::probe_type");

constexpr std::string_view probe_type_name_used(
	wrapped_type_name<probe_type>().find(probe_type_name_elaborated) != std::string_view::npos ? probe_type_name_elaborated : probe_type_name
);

constexpr size_t prefix_size() {
	return wrapped_type_name<probe_type>().find(probe_type_name_used);
}

constexpr size_t suffix_size() {
	return wrapped_type_name<probe_type>().length() - prefix_size() - probe_type_name_used.length();
}

template <typename T>
std::string_view type_name() {
	constexpr auto type_name = wrapped_type_name<T>();

	return type_name.substr(prefix_size(), type_name.length() - prefix_size() - suffix_size());
}

}
```

An example usage.

```cpp
#include <iostream>

// Snippet...

using TypeName::type_name;

int main() {
	int const *const ptr = nullptr;

	std::cout << type_name<decltype(ptr)>() << std::endl; // const int *const
}
```

## Reference

- https://stackoverflow.com/questions/81870
