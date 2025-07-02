Sure! Here's a list of C++ language changes (not standard library additions) that are introduced in C++23 — not available in C++20 — along with examples to illustrate each.

🔒 Not included: std:: library additions (e.g. std::expected, std::print, std::flat_map, etc.)
✅ Included: Core language features (syntax and semantics)

🧩 1. consteval if — Compile-Time Branching
✅ C++23 allows if constexpr to be replaced by consteval if
```cpp
consteval int square(int x) {
    return x * x;
}
```

template <typename T>
consteval int compileTimeBehavior(T val) {
    if consteval { // ✅ C++23 only
        return square(val);
    } else {
        return -1;
    }
}
❌ Not valid in C++20 — consteval if was added in C++23 to improve clarity and intent in compile-time branching.

🧩 2. explicit for Function Parameters (especially copy constructors)
✅ C++23 allows explicit on copy/move constructors with default arguments
struct S {
    explicit(false) S(const S& other = S{}) {}  // ✅ C++23: control implicitness with `explicit(bool)`
};
❌ C++20 only allows explicit or not — no boolean condition.

🧩 3. auto(x) for Function Parameters — Deduction for Copy Initialization
✅ C++23 allows auto(x) to deduce the type by copy-initialization, not template deduction rules.
void f(auto(x)) {  // ✅ C++23: deduces like `auto x = expr`
    // ...
}
Equivalent to:
template <typename T>
void f(T x);  // but with copy deduction semantics
❌ C++20 does not support auto(x) parameter declarations at all.

🧩 4. static operator() — Static Call Operator
✅ C++23 permits operator() to be declared static
struct Callable {
    static void operator()() {
        // callable without an object
    }
};

int main() {
    Callable::operator()();  // ✅ No instance needed
}
❌ In C++20, operator() must be a non-static member function.

🧩 5. multidimensional [] Operator Overloading
✅ C++23 allows overloading operator[] with multiple arguments
struct Matrix {
    int operator[](int row, int col) const {  // ✅ C++23 only
        return row * 10 + col;
    }
};
❌ C++20 allows only one argument for operator[].

🧩 6. decltype(auto) in Function Parameters (as Abbreviated Template)
✅ C++23 permits this syntax:
void foo(decltype(auto) x) {  // ✅ C++23
    // x deduced like: template<typename T> void foo(T x)
}
❌ C++20 disallows decltype(auto) as a parameter type outside of function return types or auto&&.

🧩 7. sizeof and alignof in Constant Expressions with Incomplete Types
✅ C++23 improves constant evaluation for incomplete types
template <typename T>
constexpr size_t alignment = alignof(T);  // ✅ C++23: works in more contexts even if T is incomplete
❌ C++20 restricts usage of sizeof, alignof in some constexpr contexts with incomplete types.





