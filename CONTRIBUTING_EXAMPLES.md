# CONTRIBUTING\_EXAMPLES.md (C++ Projects)

This document provides concrete examples of how to apply the **cLc doctrine** to C++ projects. The goal is to maintain a **secure, scalable, and enterprise-grade codebase**.

---

## 1. Project Structure & Build Hygiene

Always separate **headers** (`.hpp`) and **implementations** (`.cpp`).
Minimize dependencies between modules. Use **modern CMake** for reproducibility.

```cmake
# CMakeLists.txt (minimal, modern CMake)
cmake_minimum_required(VERSION 3.16)
project(SecureApp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(core STATIC src/core.cpp include/core.hpp)
target_include_directories(core PUBLIC include)
target_compile_options(core PRIVATE -Wall -Wextra -Werror -pedantic)
```

---

## 2. Immutability First

Prefer `const` and immutability. Use `constexpr` for compile-time safety.

```cpp
// ✅ Immutable
constexpr int MAX_CONNECTIONS = 100;
const std::string configPath = "/etc/secure.conf";
```

---

## 3. Small, Focused Interfaces

Headers should declare **only what’s necessary**. No logic in headers.

```cpp
// user.hpp
#pragma once
#include <string>

class User {
public:
    explicit User(std::string name);
    [[nodiscard]] std::string getName() const;
private:
    std::string name_;
};
```

---

## 4. Memory Safety

Never use raw `new/delete`. Prefer smart pointers.

```cpp
// ✅ Prefer
auto user = std::make_unique<User>("alice");

// ✅ When shared ownership is necessary
std::shared_ptr<User> sharedUser = std::make_shared<User>("bob");
```

---

## 5. Concurrency & Thread Safety

Always use RAII locks. Avoid data races.

```cpp
#include <mutex>

class SecureCounter {
public:
    void increment() {
        std::scoped_lock lock(mutex_);
        ++count_;
    }
    int get() const {
        std::scoped_lock lock(mutex_);
        return count_;
    }
private:
    mutable std::mutex mutex_;
    int count_ = 0;
};
```

---

## 6. Error Handling

Use exceptions for recoverable errors. Don’t return invalid states.

```cpp
int divide(int a, int b) {
    if (b == 0) throw std::invalid_argument("Division by zero");
    return a / b;
}
```

---

## 7. Testing & CI

Use **GoogleTest** or **Catch2**.
Enable sanitizers (`-fsanitize=address,undefined`) in CI.

```cpp
TEST(MathTests, Division) {
    EXPECT_EQ(divide(10, 2), 5);
    EXPECT_THROW(divide(10, 0), std::invalid_argument);
}
```

---

## 8. Security Practices

* **Never use unsafe C functions** (`strcpy`, `sprintf`, etc).
* Use `std::string`, `std::array`, `std::vector`.
* Compile with **security flags**:

  * `-D_FORTIFY_SOURCE=2 -fstack-protector-strong -fPIE -pie`

```cpp
// ❌ Unsafe
char buf[10];
strcpy(buf, "too long");

// ✅ Safe
std::string safe = "secure";
```

---

## 9. Declarative & Modern C++

Prefer STL algorithms and ranges over manual loops.

```cpp
std::vector<int> numbers = {1, 2, 3, 4, 5};
std::vector<int> evens;

std::copy_if(numbers.begin(), numbers.end(), std::back_inserter(evens),
             [](int n){ return n % 2 == 0; });
```

---

## 10. Code Review Expectations

Pull requests must:

* Be under **300 lines changed** (split large changes).
* Include **unit tests**.
* Build & pass sanitizers on **Linux, macOS, Windows**.
* Avoid **tech debt**: no TODOs, no commented-out code.

---

## 11. Performance & Profiling

* Do not optimize prematurely.
* Use tools: **perf, valgrind, Google Benchmark**.
* Document any optimization rationale in PR.

```cpp
// ✅ Benchmark with Google Benchmark
static void BM_StringAppend(benchmark::State& state) {
    for (auto _ : state) {
        std::string s;
        s.append("hello");
    }
}
BENCHMARK(BM_StringAppend);
```

---

## 12. Refactor Often

* Extract long functions.
* Minimize cyclomatic complexity.
* One class = one responsibility.

```cpp
// ❌ Avoid
class GodClass { /* does everything */ };

// ✅ Prefer
class UserManager {};
class PermissionChecker {};
class AuditLogger {};
```

---

## Summary

By following these practices, your C++ codebase will be:

* **Secure** (avoids UB, memory corruption, unsafe calls).
* **Maintainable** (clear, tested, modular).
* **Enterprise-ready** (cross-platform, CI/CD safe, scalable).

---
