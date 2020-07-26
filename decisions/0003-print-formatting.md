# Print Formatting

- Status: **Accepted**
- Deciders: Dan Nixon, Tristan Youngs
- Date: 24-07-2020

## Context and Problem Statement

Extensive use of C-style `printf` and friends throughout code, which requires modernisation.

## Decision Drivers

- Remove use of vulnerable C string formatting functions
- Need for more flexible print formatting (e.g. text alignment etc.)
- Require ability to handle non-POD objects (specifically `std::string` and related classes) in printed output

## Considered Options

1. [tinyformat](https://github.com/c42f/tinyformat)
2. [fmtlib](https://github.com/fmtlib/fmt)

## Decision Outcome

Chosen option: "fmtlib", because it provides more comprehensive formatting solution, and paves the way for C++20 standardisation.

### Positive Consequences

- Simplification of majority of print output calls throughout the code
- Handling of `std::string` formatting ahead of future modernisation work (see [#286](https://github.com/projectdissolve/dissolve/issues/286))

### Negative Consequences

- Lack of pre-built package for Windows systems: no Chocolatey package; no `gcc` version in Conan?

## Pros and Cons of the Options

### tinyformat

[tinyformat](https://github.com/c42f/tinyformat) is a minimal, type-safe printf() replacement.

- Good, because exists as a single header file (to be included in the project)
- Good, because provides additional formatting functions such as positional arguments
- Bad, because limited additional functionality
- Bad, because not forward looking to C++20 standard

### fmtlib

[fmtlib](https://github.com/fmtlib/fmt) is a feature-rich, type-safe replacement for printf and friends.

- Good, because offers additionsl formatting features such as field alignment and positional arguments
- Good, because it is conformant to the upcoming C++ `std::format`
- Bad, because adds an external dependency (albeit relatively well supported cross-platform)

## Links

- PR [#314](https://github.com/projectdissolve/dissolve/pull/314) re-implements the `Messenger` class, and various functions in `LineParser`, to utilise `fmtlib`.
