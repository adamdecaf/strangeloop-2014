# Type Systems - The Good, Bad and Ugly

## The Good

__What makes a type system good?__

- Curry-Howard
  - Types are Theorems
  - Implementations are Proofs
- Lots of powerful types

## The Bad

__What makes a type system bad?__

- Stringly typed systems
- Lacking connections betwen logic and code

## The Ugly

- Horribly complex type signatures and code
- False negatives
  - Having to cast a value's type to let the compile pass

## What makes a language with a good type system good?

- Language features that leverage the type system.
  - Pattern Matching
  - Product Types
    - *cough* lenses *cough*
  - Currying
  - Dependent Types
