# The Road to Akka Cluster, and Beyond

## Current

- We all have distributed systems, there's no escaping it
  - But that's a good thing
- We're trying to overcome some things
  - The speed of light
  - independent things fail independently

## Problems

- The network is bad
- You can't really tell the difference between:
  - a dead node
  - a slow node
- Holding state in memory is horrible

## Goals

- Should allow for expressive handling of
  - Concurrency
  - Distribution
  - Mobility

## Possible Solutions

- lambda calculus natively supports concurrency
  - b-reduction allows evaluation in any order
  - easy to then distribute computations
- actors
  - share nothing outside of actor
  - operations are atomic within an actor
  - async message passing across actors
    - non-determinism in message delivery

## Impossibility Theorems

- FLP (Fischer Lynch Patterson 1985)
  - Consensus is impossible
    - There are pragmatic ways around this actually
- CAP Theorem
  - linearizability is impossible
    - within certain constraints
    - it's often not required though
    - ignores latency
  - a read will return the last completed write
  - why do we have to always let go of C or A _all of the time_?

## Time

- fundamental concept in distributed systems
- Lamport clocks (1978)
  - when a process does work, increment
  - when a process sends a message, include the counter
  - when a process gets a message, merge the counters with `max()`
- Vector clocks
  - each node owns and increments it's own lamport clock
    - `[node -> lamport clock]`
  - always keep full history of increments
  - merges by calculating `max()` -- monotonic merge
    - will always converge

## Failure Detection

- __NOT__ black and white
- Strong Correctness
  - every crashed process is eventually suspected by _every correct_ process
- Weak Correctness
  - every crashed process is eventually suspected by _some correct_ process
- Strong Accuracy
  - no correct process is _ever suspected_
- Weak Accuracy
  - some correct process is _never suspected_
- Accrual failure detector (2004)
  - history of heartbeats, calculates a likelyhood that a process is down
  - accounts for network hiccups
    - could be suspected because of having to GC, not purely a network issue
- SWIM failure detector (2002)
  - partitions heartbeats from cluster dissemination
  - quarantine: suspected => time window => faulty
    - not a hard and fast removal from cluster
    - could be suspected because of having to GC, not purely a network issue
  - delegated hartbeat
    - ask a friend to check the health of another process
    - let's you calculate the network dedregation between individual nodes
      - can route around those during the dedregation

## Consistency

- Dynamo (2007)
  - Eventual Consistency
  - Popularized: hinted handoffs, consistent hashing, etc..
- Epidemic Gossip
  - Leverages CHORD to make a ring of members
  - Then send messages from random member to another
    - Can use probabilistic source for choosing members
    - Similar to how actual vriuses spread
  - No leader required
  - Easy / fast to gain consensus

## Akka

- Stack (in order, bottom up)
  - Akka Cluster Extensions
  - Akka Cluster
  - Akka Remote
  - Akka io
  - Akka Actors

__Akka Cluster__

- Features
  - Membership
  - Leader & Singleton
  - Cluster Sharding
  - Clustered Routers
  - Supervision / Deathwatch
  - Pub/Sub
  - etc..
- Fast converging
  - 4 mins w/ 1000 nodes
  - endemic gossiping

__Gossiping__

```scala
case class Gossip( // is a CRDT
  members: SortedSet[Member], // ordered node ring
  seen: Set[Member],
  unreachable: Set[Member],
  version: VectorClock
)
```

- Biased Gossiping
- Push/Pull
  - When we have convergance we can only send version
    - When ^ fails, just send the whole Gossip message

## The future

- Akka 2.4
  - Akka Http
  - Akka Streams
- Akka 2.x?
  - Akka CRDT
  - Akka Raft
