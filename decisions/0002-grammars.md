# Grammar Generation (Parser / Lexers)

- Status: **Proposed**
- Deciders: Dan Nixon, Adam Washington, Tristan Youngs
- Date: 25-07-2020

## Context and Problem Statement

Necessary grammars in Dissolve (e.g. for mathematical expression handling) are encompassed in parser/lexers generated with [Bison](https://www.gnu.org/software/bison/). Bison is stable but no longer actively developed, and is considered as a legacy code in this context.

## Decision Drivers

- Bison is legacy software: stable, but no longer actively maintained
- Bison C++ support is of limited functionality, and does not permit modern routes to its incorporation into the parent code

## Considered Options

1. [ANTLR](https://www.antlr.org/)

## Decision Outcome

Chosen option: "ANTLR", because it is the only modern alternative available.

### Positive Consequences

- Clearer definition of grammars (use of regular expressions, token validator functions etc.)
- No custom lexer required
- Better C++ integration between parser and 'real' code

### Negative Consequences

- ANTLR4 has a separate C++ runtime library that is not widely available on all platforms (e.g. Windows, Ubuntu 18.04), requiring additional effort to ensure that the dependency is available at build time (in both user and CI spaces)

## Pros and Cons of the Options

### ANTLR

[ANTLR](https://www.antlr.org/) (current version 4.8 at time of writing) is a modern parser generator with good C++ support.
[ANTLR4 on GitHub](https://github.com/antlr/antlr4)

- Good, because it is actively maintained and well-supported
- Good, because its main Java component is available on all major architectures
- Good, because it offers useful capability beyond Bison (e.g regular expressions)
- Good, because it has superior C++ support to Bison
- Bad, because the C++ runtime library is potentially unavailable in package repositories for some sytems

## Links

- PR [#312](https://github.com/projectdissolve/dissolve/pull/312) implements the NETA grammar in ANTLR4.
