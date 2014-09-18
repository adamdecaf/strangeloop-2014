# Pauseless GC on the jvm

## Why do we need another GC?

- Each GC is specialized for a unique use case.

## What are the goals?

- "<10ms GC pause times for 100gb+ heaps"
- Shenandoah is really about minimal pause times.

## Why is it hard?

- Concurrent moving of objects from the heap is hard.
  - Everyone has to agree at the same time.
- Stopping the world really sucks.
- Writing an object in a from-region will trigger an evacuation
  - So, writes can never occur in from-regions

## What does it solve?

- Smallest pauses possible.
- Done with concurrent marks, small pauses, and concurrent evacuation
- Doesn't have to evacuate upon read (like other GC's)

## Concurrent marking

- Snapshot at the beginning
  - anything live at init mark is considered live
  - anything allocated after init is considered live
