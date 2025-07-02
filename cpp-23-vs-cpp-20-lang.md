Sure! Here's a list of C++ language changes (not standard library additions) that are introduced in C++23 — not available in C++20 — along with examples to illustrate each.

🔒 Not included: std:: library additions (e.g. std::expected, std::print, std::flat_map, etc.)
✅ Included: Core language features (syntax and semantics)

🧩 1. `consteval if` — Compile-Time Branching
✅ C++23 allows if constexpr to be replaced by consteval if
```cpp
consteval int square(int x) {
    return x * x;
}

template <typename T>
consteval int compileTimeBehavior(T val) {
    if consteval { // ✅ C++23 only
        return square(val);
    } else {
        return -1;
    }
}
```
❌ Not valid in C++20 — `consteval if` was added in C++23 to improve clarity and intent in compile-time branching.

🧩 2. explicit for Function Parameters (especially copy constructors)
✅ C++23 allows explicit on copy/move constructors with default arguments
```cpp
struct S {
    explicit(false) S(const S& other = S{}) {}  // ✅ C++23: control implicitness with `explicit(bool)`
};
```
❌ C++20 only allows explicit or not — no boolean condition.

🧩 3. auto(x) for Function Parameters — Deduction for Copy Initialization
✅ C++23 allows auto(x) to deduce the type by copy-initialization, not template deduction rules.
```cpp
void f(auto(x)) {  // ✅ C++23: deduces like `auto x = expr`
    // ...
}
```
Equivalent to:
```cpp
template <typename T>
void f(T x);  // but with copy deduction semantics
```
❌ C++20 does not support auto(x) parameter declarations at all.

🧩 4. static operator() — Static Call Operator
✅ C++23 permits operator() to be declared static
```cpp
struct Callable {
    static void operator()() {
        // callable without an object
    }
};

int main() {
    Callable::operator()();  // ✅ No instance needed
}
```
❌ In C++20, operator() must be a non-static member function.

🧩 5. multidimensional `[]` Operator Overloading
✅ C++23 allows overloading `operator[]` with multiple arguments
```cpp
struct Matrix {
    int operator[](int row, int col) const {  // ✅ C++23 only
        return row * 10 + col;
    }
};
```
❌ C++20 allows only one argument for `operator[]`.

🧩 6. `decltype(auto)` in Function Parameters (as Abbreviated Template)
✅ C++23 permits this syntax:
```cpp
void foo(decltype(auto) x) {  // ✅ C++23
    // x deduced like: template<typename T> void foo(T x)
}
```
❌ C++20 disallows `decltype(auto)` as a parameter type outside of function return types or `auto&&`.

🧩 7. `sizeof` and `alignof` in Constant Expressions with Incomplete Types
✅ C++23 improves constant evaluation for incomplete types
```cpp
template <typename T>
constexpr size_t alignment = alignof(T);  // ✅ C++23: works in more contexts even if T is incomplete
```
❌ C++20 restricts usage of `sizeof`, `alignof` in some `constexpr` contexts with incomplete types.

🧩 8. Deduction Guides for `explicit`-specified Constructors
C++23 allows deduction guides for constructors marked `explicit`.
```cpp
struct Wrapper {
    explicit Wrapper(int) {}
};

Wrapper w1(42);              // OK
Wrapper w2 = Wrapper{42};    // OK
Wrapper w3{42};              // OK

// Template deduction works in C++23 with explicit constructor:
template<typename T> struct Box {
    explicit Box(T) {}
};

Box b1(123);      // OK in C++23
// Error in C++20: no matching constructor due to `explicit`
```
✅ C++23: accepts this code
❌ C++20: rejects due to `explicit`

🧩 9. `auto x = [capture]{...};` Lambdas with `= this`
C++23 adds `= this` capture for clarity and safety.
```cpp
struct MyClass {
    int val = 100;

    auto get_lambda() {
        return [= this] { return val; };  // captures this explicitly
    }
};
```

🧩 10. `constexpr` for `try/catch` Blocks
C++23 allows `try/catch` inside `constexpr`.
```cpp
constexpr int test(int x) {
    try {
        if (x == 0) throw "zero";
        return 100 / x;
    } catch (...) {
        return -1;
    }
}

static_assert(test(0) == -1);  // ✅
```
✅ C++23: works
❌ C++20: `try/catch` not allowed in `constexpr`

🧩 11. Default Member Initializers in Unions
C++23 removes restriction: unions can have default initializers.
```cpp
union U {
    int a = 42;
    float b;
};

U u;  // u.a is initialized to 42
```
✅ C++23: OK
❌ C++20: Error – unions can't have default member initializers

🧩 12. `static_assert` Without Message (in non-constant contexts)
C++23 allows `static_assert(expr);` without a message outside of constant evaluation.
```cpp
void f(int x) {
    static_assert(x > 0);  // OK in C++23 even if x is runtime
}
```
✅ C++23: valid
❌ C++20: requires a message if the expression is non-constant
