# CONTRIBUTING\_EXAMPLES.md

*(for C++ Projects)*

This document shows practical **C++ code examples** for each of the core contributing principles.
All contributors should aim to follow these practices to ensure code quality, security, and maintainability.

---

## 1. Write Clean & Readable Code

Keep code expressive, simple, and consistent.

```cpp
// ❌ Bad
int f(int a, int b) { return a+b; }  

// ✅ Good
int AddTwoNumbers(int lhs, int rhs) {  
    return lhs + rhs;  
}
```

---

## 2. Prefer Immutability

Avoid unnecessary mutations. Use `const` wherever possible.

```cpp
// ❌ Bad
std::string name = "Alice";
name = "Bob";  

// ✅ Good
const std::string name = "Alice";
```

---

## 3. Modular Design

Break complex logic into smaller reusable functions/classes.

```cpp
// ❌ Bad
void ProcessData(const std::vector<int>& data) {
    // too much logic in one function
    for (int x : data) { /* … */ }
    // extra unrelated operations here
}

// ✅ Good
void ValidateData(const std::vector<int>& data);
void NormalizeData(std::vector<int>& data);
void SaveData(const std::vector<int>& data);

void ProcessData(const std::vector<int>& data) {
    ValidateData(data);
    auto normalized = data;
    NormalizeData(normalized);
    SaveData(normalized);
}
```

---

## 4. Favor Composition Over Inheritance

Prefer combining objects instead of deep inheritance.

```cpp
// ❌ Bad
class Bird {
public: virtual void Fly() = 0;
};

class Penguin : public Bird {
public: void Fly() override {} // Penguins don’t fly — misleading!
};

// ✅ Good
class SwimmingAbility {
public: void Swim();
};

class Penguin {
private:
    SwimmingAbility swim_;
public:
    void Swim() { swim_.Swim(); }
};
```

---

## 5. Explicit Resource Management (RAII)

Always manage resources safely (files, sockets, memory).

```cpp
// ✅ Good
void ReadFile(const std::string& path) {
    std::ifstream file(path); // RAII ensures file closes automatically
    if (!file) throw std::runtime_error("Cannot open file");
}
```

---

## 6. Write Unit Tests

Every function should be tested. Use frameworks like **GoogleTest**.

```cpp
// Example with GoogleTest
#include <gtest/gtest.h>

int Add(int a, int b) { return a + b; }

TEST(MathTests, AddTwoNumbers) {
    EXPECT_EQ(Add(2, 3), 5);
}
```

---

## 7. Refactor Continuously

Prefer clarity over cleverness.

```cpp
// ❌ Bad
int a(int b,int c){return b^c;} // unclear

// ✅ Good
int XOR(int lhs, int rhs) {
    return lhs ^ rhs; // descriptive
}
```

---

## 8. Handle Errors Gracefully

Never ignore errors, return results explicitly or throw exceptions.

```cpp
// ❌ Bad
int Divide(int a, int b) { return a / b; } // crash if b == 0  

// ✅ Good
std::optional<int> SafeDivide(int a, int b) {
    if (b == 0) return std::nullopt;
    return a / b;
}
```

---

## 9. Favor Declarative over Imperative

Express *what* needs to be done, not *how* step-by-step.

```cpp
// ❌ Imperative
std::vector<int> numbers = {1,2,3,4,5};
std::vector<int> evens;
for (int n : numbers) {
    if (n % 2 == 0) evens.push_back(n);
}

// ✅ Declarative (C++20 ranges)
auto evens = numbers
    | std::views::filter([](int n) { return n % 2 == 0; });
```

---

## 10. Document as You Code

Provide clear inline comments and API docs.

```cpp
/// Adds two integers and returns the result.
/// @param lhs Left-hand side integer
/// @param rhs Right-hand side integer
/// @return Sum of lhs and rhs
int Add(int lhs, int rhs);
```

---

✅ Following these examples will keep the codebase **scalable, secure, and easy to maintain** — similar to projects like **cURL** or **FFmpeg**.

---
