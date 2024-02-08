# Distributed Systems What and Why

## Philosophy of solving problem in computer science
1. Distributed Systems
   - treat partial failure as expected and work around it

2. High Performance Computing (HPC)
   - treat partial failure as total failure
   - peroidically create checkpoint/snapshot for failure recover

## What is distributed systems
- Connected computers work together to solve problems and handle ***failures*** and ***unbounded latency***.

### Common failures in distributed systems
- physical damage
- network connetivity
- all kind of latency
- data corruption
- bugs

> 💡 It is ***impossible*** to identify the failure in most of time

## Why
- make things faster (throughput/computation power)
- you have no choice
- reliability (replicas)