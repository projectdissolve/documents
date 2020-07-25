# C++ Standard

- Status: **Accepted**
- Deciders: Lamar Moore, Dan Nixon, Adam Washington, Tristan Youngs
- Date: 15-06-2020

## Context and Problem Statement

The project to date uses little functionality of modern C++ standards, owing to its use of custom classes for list, object management etc. C++11 is required for Qt5 support. Modernisation of the project requires the use of newer C++ features than those available in C++11.

## Decision Drivers

- Need for modernisation - e.g. removal of custom classes in favour of STL replacements
- Replacement of raw pointer usage with references where possible, requiring the use of `std::optional` (from C++17)

## Considered Options

1. Move to C++17
2. Move to C++20

## Decision Outcome

Chosen option: "C++17", because it provides all features necessary for current modernisation efforts.

### Positive Consequences

- Ability to reduce raw pointer usage in favour of references, especially in cases where a getter may need to return no object (using `std::optional<std::reference_wrapper<T>>`)
- Preparedness for integrating next major version of Qt (version 6) which will rely on C++17

### Negative Consequences

- Enforcing C++17 breaks generation of AppImages via [AppImageKit](https://github.com/AppImage/AppImageKit), which enforces building on the oldest LTS Ubuntu version (at time of writing 16.04) whose system `gcc` does not support C++17

## Pros and Cons of the Options 

N/A.

## Links

- PR [#293](https://github.com/projectdissolve/dissolve/pull/293) moves the code to C++17 use
