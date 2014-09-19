# Consistency without consensus in production systems

## Idioms

__1908's RPC's__

- Model network communication with a function call

__2000 CAP___

- Partition Tolerance
  - System continue to operate despite message loss or unavailability
- CP Systems
  - Strong consistency up to a threshold of failure
  - Based on consistency protocols
    - Paxos (1989), Zab (2008), Raft (2013), Viewstamped Replication (1988)
- AP Systems
  - Used by most distributed data systems
    - mongo, cassandra, riak, couch
  - Often just "less than strong consistency"
    - "system will converge, eventually"

__CALM Principle__

- "Consistency as Logical Monotonicity"
- As a system gets more messages, it should only grow in one dimension.
  - Example language: `bloom language`

__ACID 2.0__

- Associative, Commutative, Idempotent, Distributed

__CRDT__

- Conflict-free replicated data type
- Set's are a good example
  - Let's us fix the problem over time, given some inconsistency

## Failure

__How can messages fail?__

- Delayed
- Dropped
- Delivered out of order
- Duplicated

## Examples

__Addition__

[x] Communative
[x] Associative
[ ] Idempotent

__Set Unions__

[x] Communative
[x] Associative
[x] Idempotent

## Use Cases

- Fan out on write
  - When an event happens write it to each inbox that could consume it
- Fan in on read
  - When a read happens, collect events from outboxes

## Roshi Set

- A single outerfacing set is represented by two sets
  - Write and delete sets, merged on read

## Soundcloud example

- [github.com/soundcloud/roshi](github.com/soundcloud/roshi)

```
 [r] [r] [r]   [r] [r] [r]
--- pool ---- --- pool ----
-- cluster -- -- cluster --
---------- farm -----------
```

- Apps write to the farm, in which the farm duplicates the request to all clusters.
- The keys are split up in the pool (name hashing)
- Writes just need a quorum (typically > 51%)
- When a cluster dies, add a new one to the farm.
