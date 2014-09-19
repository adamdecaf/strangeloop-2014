# Mesos: The Operating System for your Cluster

## History

- Why don't we all share the same computer at the same time?
  - Requires a _big_ computer
    - `s/computer/cluster/ig`

## What is it

- Isolation
- Smart scheduling for tasks
- Common interface, but at task level

## How can I use it?

1. `apt-get install -y mesos`
1. `/etc/init.d/mesos-master start`
1. `/etc/init.d/mesos-slave start` x 1,000,000,000
1. boom!

## How do I build on it.

- write / use frameworks
- ship containers
