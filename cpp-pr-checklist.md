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
