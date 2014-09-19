# Function Passing Style: Typed, Distributed Functional Programming

## Goals

- A better programming model
  - fault-tolerance
  - in-memory caching
  - debugging

## Invert the actor model

- functions are exchanged through async messaging
  - keep the data in place
- accept the fundamental differences between local and distributed computing

## What's it like?

- Silos
  - `Silo[T]`
  - `SiloRef[T]`
    - `def apply(s1: Spore, s2: Spore): SiloRef[T]`
    - `def send(): Future[T]`
- Spores
  - Create a closure to send along.
  - Uses macros to ensure there's nothing outside of the spore's scope.
    - Is there a way to lessen this restriction? safely?
    - How are you going to handle `CanBuildFrom[_, _ , _]`?
      - Perhaps using something like transducers to generalize over all collection types

## Example

```
SiloRef[List[Int]]
  -> .apply(map, f)
    -> .apply(map, (_ * 2))
      -> .apply(reduce, (_ + _))
        -> .send()
```

## Fault tolerance

- Can easily re-run failed chains
  - since everything is immutable
- Also, can spead out intermediate steps
  - persistable

## Papers

- "A note on Distributed Computing"
  - Jim Waldo
- [sips/sportes.html](http://docs.scala-lang.org/sips/pending/spores.html)
