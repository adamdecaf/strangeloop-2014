# Ludicrous Speed: Designing For Performance on the JVM

## Relative Times

> 1ns = 1mi

- Registers: Brisk Walk
- L1 Cache: Nice bike ride
- L2 Cache: Suburban commute
- Main Memory: A day trip
- HDD: Literally going to mars
- Network: Good luck starchild

## Java's False Trade

- Objects are only referenceable from pointers
- Generic collections is a collection of types.
- So you're forced to choose:
  1. Fast and brittle
  1. Slow and safe.

## Tools

- HPPC
  - Collections ware with cross product of all primive types
  - Breaks API with (j.u.*) Collections
- FastTuple
  - Hot code loading, new class defs at runtime
  - Has code generator to compile schemas to delay finding static types until runtime

## Actors

- Java is really meant for a 1-1 relation with threads.
- ThreadPools that still have to be benchmarked and scaled properly.
  - Dynamic scaling?

## Microbenchmarking

- Use JMH
  - Need to warm up the JIT for best performance
  - Annotation processor based, build a fat jar
  - Very customizable, lots of reporting features

## JIT

- Short, in-linable methods are best
- Avoid polymorphic dispatch from the same call site
- Sometimes the JVM will alloc objects on the stack, as long as it doesn't escape the stack
- There are also lots of calls that are straight to assembly.
  - `PrintAssembly` and `PrintInlining` flags to set
