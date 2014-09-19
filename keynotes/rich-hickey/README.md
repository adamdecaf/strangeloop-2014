# Transducers

## Basics

- not a new idea, just a new usage
- pull out the essence of map, filter, etc
  - so that any new collection is totally supported
- away from functions that transform collection
- so they can be used elsewhere
  - "concrete reuse"
- recast them as process transformations

__What kinds of processes?__

- ones that are a succession of steps
  - where each step ingests input
- seeded left reduce is the generalization

## They already exist

- "put the baggage on the plane"
  - as you're doing that
    1. break apart the pallets
    1. remove bags that smell like food
    1. label heavy bags
- the sources and sinks are _irrelevant_
  - telling where the inputs come from overly complicates the problem
    - "I don't have rules for that"
- the rules however, filter, map, etc only apply to specific types
  - no true generalization, yet..

## Creating Transducers

```clojure
;; this is really what we want
(def process-bags
  (comp
   (mapcatting unbundle-pallet)
   (filtering non-food?)
   (mapping label-heaby))
```

- `mapping`, `filtering`, and `mapcatting` all return transducers
- processes that can use this become a lot more powerful

## How to build them

- Parameterize out the essence of the hidden steps from `mapping`, `filtering`, etc..
- Can then defer the step until the end.
  - Lets us have perf optimizations

```clojure
(defn map [f xs]
  (fn [step]
    (reduce f (cdr xs) (step (car xs)))))
```

## Advantages

- Just a stack of function calls
- No laziness, interim collections
- No boxing
- Early termination
  - clojure has a `reduced` method to handle this

## Transducer Types

- Current area of research
- Pseudo-type: `((_^N-1, A) => _^N) -> (_^M-1, B) => _^M`
- Will need to encode never calling `step` once a `reduced` is encountered.
  - Consumers of reducers must never ask for more once they're given reduced
    - `(((_^N-1, A) => _^N) | Reduced(_)^N) -> (((_^M-1, B) => _^M) | Reduced(_)^M)`

## State

- Some transducers require state.
  - `take`, `partition`
- When transducers are applied to a process it yields another (probably stateful) process.
  - which should not be aliased
- Pass transducers around and let processes apply them.

## Init

- airty-0 function required to give an initial accumulator value
- transducers must support airty-0 operation
