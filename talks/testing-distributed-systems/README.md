# Testing Distributed Systems

- We all know that it's "hard" test distributed systems.
- Lots of ways to fail.

## Simulations

- Don't design software. Design a simulation.
  - Interesting choices to hide away the actual problems.
- Wire together linear, predictable systems to setup your simulation.
  - Then you can write DR simulations for specific failures.
- "Our simulations are only as good as our understanding of the OS and hardware."

## Flow

- C++ extension that makes "actors" with single threaded callbacks

## Situations you can setup

- dumb sysadmins
- failing machines
- datacenter loss
- HDD swaps
- give bad dates
- networking failures
- etc..

## What can we do?

- Docker container to wrap others and simulate failure.
  - Can create entire stacks and have deterministic failures happen.
  - Pause / restart containers. (Simulate hard power failures.)
