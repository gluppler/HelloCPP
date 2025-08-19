# CONTRIBUTING\_EXAMPLES.md (C++)

## Contribution Style and Principles

This document defines the **coding style, best practices, and contribution workflow** for C++ projects in this repository. It ensures that code remains **scalable, maintainable, and enterprise-grade**, similar to projects like **cURL** or **FFmpeg**.

---

## 🔑 Core Principles with C++ Examples

### 1. **Functional Thinking (Where Possible)**

Prefer pure functions, immutability, and minimal side effects.

```cpp
// Avoid (stateful, side-effect heavy)
int counter = 0;
int increment() { return ++counter; }

// Prefer (stateless, functional style)
int increment(int value) { return value + 1; }
```

---

### 2. **RAII (Resource Acquisition Is Initialization)**

Use RAII for memory, files, and locks to prevent leaks.

```cpp
#include <fstream>

void writeToFile(const std::string& path) {
    std::ofstream file(path); // Automatically closed at scope end
    file << "Hello, World!";
} // file closed safely here
```

---

### 3. **Smart Pointers > Raw Pointers**

Avoid manual memory management unless absolutely required.

```cpp
#include <memory>

std::unique_ptr<int> value = std::make_unique<int>(42);
```

---

### 4. **Unit Tests for Each Feature**

Use frameworks like **GoogleTest** or **Catch2**.

```cpp
#include <gtest/gtest.h>

int add(int a, int b) { return a + b; }

TEST(MathTest, Add) {
    EXPECT_EQ(add(2, 3), 5);
}
```

---

### 5. **Refactor for Clarity**

Keep functions short, modular, and descriptive.

```cpp
// Avoid
void process(std::vector<int>& v) { /* does everything */ }

// Prefer
void normalize(std::vector<int>& v) { /* single responsibility */ }
void filter(std::vector<int>& v) { /* single responsibility */ }
void process(std::vector<int>& v) {
    normalize(v);
    filter(v);
}
```

---

### 6. **Error Handling: Exceptions and Expected**

Use exceptions for critical failures, `std::optional` / `std::expected` for predictable cases.

```cpp
#include <optional>
#include <string>

std::optional<std::string> findUser(int id) {
    if (id == 1) return "Alice";
    return std::nullopt;
}
```

---

### 7. **Follow the Rule of 5/0**

If you manage resources, implement the **Rule of 5**. Otherwise, use default (Rule of 0).

```cpp
class Resource {
public:
    Resource() { data = new int[10]; }
    ~Resource() { delete[] data; }

    // Rule of 5
    Resource(const Resource& other);            // Copy ctor
    Resource& operator=(const Resource& other); // Copy assign
    Resource(Resource&& other);                 // Move ctor
    Resource& operator=(Resource&& other);      // Move assign
private:
    int* data;
};
```

---

### 8. **Namespaces and Modules**

Keep code modular and avoid global pollution.

```cpp
namespace math {
    int square(int x) { return x * x; }
}
```

---

### 9. **Performance with Safety**

Use `std::vector` and algorithms instead of raw arrays and loops.

```cpp
#include <vector>
#include <algorithm>

std::vector<int> v = {1, 2, 3};
std::for_each(v.begin(), v.end(), [](int& n) { n *= 2; });
```

---

### 10. **Consistent Style**

* Follow **Google C++ Style Guide** (or project’s chosen guide).
* Use `clang-format` for formatting.
* Keep naming consistent (`CamelCase` for types, `snake_case` for variables/functions).

```cpp
class DataProcessor {
public:
    void process_data();
};
```

---

## ✅ Contribution Checklist

Before submitting PRs:

* [ ] Write **unit tests** for new features/bug fixes.
* [ ] Run `clang-format` on all code.
* [ ] Avoid raw pointers unless unavoidable.
* [ ] Keep functions **<50 lines**.
* [ ] Document new public APIs with comments.

---

This version is **expanded, enterprise-ready, and beginner-friendly** — showing **practical, modern C++ examples** while aligning with the **doctrine** we’ve set.

---
