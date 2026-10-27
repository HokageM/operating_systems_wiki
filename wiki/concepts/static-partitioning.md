---
title: Static Partitioning
type: concept
tags: [memory, allocation]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Static Partitioning

## Definition

**Static partitioning** (statische Partitionierung) — *variant 1* of memory allocation in the
lecture — divides main memory into **partitions of fixed size**, decided in advance. The
operating system occupies one partition; every other process is **permanently assigned exactly
one** partition for its lifetime.

## Context

Belongs to [[memory-management]]. It is the simplest allocation scheme and the baseline against
which [[dynamic-partitioning]] (variant 2) and [[paging]] (variant 2a) are judged.

## How it works

```
+-----------+------------+------------+------------+
|    OS     | partition 1| partition 2| partition 3|
+-----------+------------+------------+------------+
```

Sizes and boundaries are fixed at system configuration time. A process is loaded into one
partition and stays there. [[address-translation]] is therefore a single addition of the
[[base-and-limit-register|base register]], and protection is a single comparison against the
limit register.

### Trade-offs

| | |
|---|---|
| (+) | **simple address translation** — one addition, one comparison |
| (+) | trivial bookkeeping: a partition is free or occupied |
| (−) | the **maximum number of active processes is limited** by the number of partitions |
| (−) | partitions are **not fully used** — a process smaller than its partition wastes the rest |

The second disadvantage is exactly **internal [[fragmentation]]**.

## Related

- [[dynamic-partitioning]] — the answer to both disadvantages, at the cost of external
  fragmentation.
- [[fragmentation]] — internal fragmentation is inherent here.
- [[base-and-limit-register]] — the protection mechanism.
- [[paging]] — the scheme that finally removes external fragmentation entirely.

## Common confusions

- **"Fixed size" does not mean "all the same size".** The partitions are fixed, not necessarily
  equal; a system may offer a few small and a few large ones. What is fixed is that they do not
  change at runtime. (Equal *and* fixed sizes are the property of frames under [[paging]].)
- **The limit on active processes is a limit on *resident* processes.** More processes can exist;
  they simply have to be swapped out (see [[swapping]]).

## Sources

- [[lecture-bs-4]] (p04)
