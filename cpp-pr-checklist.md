# C++ PR checklist

## Naming

- [ ] Members of a class should be prefixed with a `m_`
- [ ] Static variables should be prefixed with a `s_`
- [ ] Struct names should be generic enough to encompass all inside parameters logically

## Classes

- [ ] No unused methods should be left
- [ ] Method parameter types should ensure type safety. Always use types that require the least amount of casting. Unless it's super inconvenient to use the API.
- [ ] If a function takes no parameters, use empty parameter list (keyword `void` can be omitted).
- [ ] Check whether class should support instancing or whether it could be simply made static
- [ ] If a class has a setter, it should also have a constructor accepting that value as parameter

## Iterators

- [ ] When using `std::map`, to find an element use `find()` and handle no result appropriately. It's safer than `at()`.

## Errors

- [ ] Functions/methods that throw should be handled appropriately (either adding error checking or checking that logic never throws)
- [ ] `ASSERT` for a bad input is not enough. We should make sure no crash happens after assert as well.

## General

- [ ] All necessary `#include`'s for a file should be added
