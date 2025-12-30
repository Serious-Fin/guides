# C++ PR Checklist

## Naming & Conventions
- [ ] Class member variables are prefixed with `m_`
- [ ] Static class variables are prefixed with `s_`
- [ ] Struct/class names are descriptive and logically represent their contents
- [ ] Naming follows team conventions consistently

## Class & API Design
- [ ] Unused methods are removed
- [ ] Parameter types avoid unnecessary casting; choose the safest, most specific type
- [ ] Functions with no parameters use an empty parameter list (`void` omitted)
- [ ] Determine whether the class should be instanced or made static
- [ ] If a class has a setter for an essential value, the constructor also accepts it
- [ ] Use `const` for variables, parameters, and methods when mutation is not required
- [ ] Use references on complex types to avoid copying

## Iterators & STL Usage
- [ ] When checking key existence in `std::map`, use `find()` and handle the “not found” case
- [ ] Avoid unsafe use of `at()` unless bounds checking is explicitly intended

## Error Handling & Safety
- [ ] Throwing functions have proper error handling (catch, guard, or ensure non-throwing logic)
- [ ] `ASSERT` is considered where appropriate
- [ ] `ASSERT` alone is not sufficient—ensure no crash occurs afterward
- [ ] All paths avoid undefined behavior (null checks, bounds checks, ownership correctness)

## Includes & Dependencies
- [ ] All required headers are included
- [ ] Unnecessary includes are removed

## Task Acceptance / Requirements
- [ ] Acceptance criteria for the work item/story is defined for QA

## Book advice
- [ ] Prefer `auto` to explicit type declarations
- [ ] When casting to different type, prefer explicit `static_cast` for clarity.
- [ ] For object ctor use brace syntax (`obj{...}`) for most widely used initialization syntax and parentheses syntax (`obj(...)`) for handling `std::initializer_list` params correctly.
- [ ] Prefer `nullptr` to `0` and `NULL`. Because `nullptr` can't be converter to any integral implicitly. Also improves code clarity.
- [ ] Prefer `alias` declarations to `typedef`s. Alias syntax is simpler and supports templatization.

Using `typedef` syntax:
```cpp
typedef
    std::unique_ptr<std::unordered_map<std::string, std::string>>
    UPtrMapSS;
```

Using `alias` syntax:
```cpp
using UPtrMapSS =
    std::unique_ptr<std::unordered_map<std::string, std::string>>;
```

- [ ] Prefer scoped enums to unscoped enums.

Using unscoped enums:
```cpp
enum Color { black, white, red };
auto white = false; // error! white already declared in this scope
```

Using scoped enums:
```cpp
enum class Color { black, white, red };
auto white = false; // Ok
```

- [ ] Prefer `const_iterator` to `iterator`
- [ ] Declare functions `noexcept` if they won't emit exceptions. `noexcept` is particularry valuable for move operations, `swap`, deallocations functions and destructors.
- [ ] Use `constexpr` whenever possible.
- [ ] Ensure thread safety in member functions when those are used in concurrent contexts using `atomic` for simpler cases and `mutex` for more complex cases.
- [ ] Use `std::unique_ptr` for exclusive-ownership resource management.
- [ ] Use `std::shared_ptr` for shared ownership resource management.
- [ ] Use `std::weak_ptr` for `std::shared_ptr`-like pointers that can dangle and code should be able to detect that dangling.
- [ ] Prefer `std::make_unique` and `std::make_shared` to direct use of `new`.
- [ ] Avoid default capture modes in lambda expressions.

Default by-reference capture can lead to dangling references.
```cpp
void addDivisorFilter()
{
    auto calc1 = computeSomeValue1();
    auto calc2 = computeSomeValue2();
    auto divisor = computeDivisor(calc1, calc2);
    filters.emplace_back(                               // danger!
        [&](int value) { return value % divisor == 0; } // ref to
    );                                                  // divisor
} 
```

Default by-value capture is susceptible to dangling pointers (especially `this`), and it misleadingly suggests thatlambdas are self-contained.
```cpp
void addDivisorFilter()
{
    static auto calc1 = computeSomeValue1();            // now static
    static auto calc2 = computeSomeValue2();            // now static
    static auto divisor = computeDivisor(calc1, calc2); // now static

    filters.emplace_back(
        [=](int value)                      // captures nothing!
        { return value % divisor == 0; }    // refers to above static
    );
    ++divisor;  // modify divisor
}
```

- [ ] Use `init` capture to move objects into closures in lambda expressions (C++14 and up)

```cpp
class Widget {  // some useful type
public:
    …
    bool isValidated() const;
    bool isProcessed() const;
    bool isArchived() const;

private:
    …
};

auto pw = std::make_unique<Widget>();
…   // configure *pw
auto func = [pw = std::move(pw)]                // init data mbr
            { return pw->isValidated()          // in closure w/
                     && pw->isArchived(); };    // std::move(pw)
```

- [ ] Prefer lambdas to `std::bind`

(chapter 7, p259)

## To ask:
- Do we prefer `auto` in our code base if it can be used?
- Is there a standard way of initialization that we should use? Braces, parenmtheses?
