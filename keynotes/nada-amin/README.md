# Programming should eat itself

> Nada Amin

## Meta Levels

- Programming is just building upon the previous layer
  - Can then decompose everything as various meta levels
- Black (language) is an example of this concept
- Basically being able to manually create lexical scoping
  - Complete with isolated file loading

## Tracing

- Given that you can parse the running ast, you can easily trace all the calls
  - Easy to implement instruction counting
  - Or run-time macros

## TABA

> _There and Back Again_

- Ex: Zipping a list with the reverse of another
  - In `Θ(n)` time for both however
- Walk down first list with the unwrap of the other
