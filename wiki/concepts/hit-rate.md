---
title: Hit Rate
type: concept
tags: [memory, caching, performance, exam]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Hit Rate

## Definition

The **hit rate** (Trefferrate) is the **share of accesses that can be served by a fast level of
the [[memory-hierarchy]]** — for the [[translation-lookaside-buffer]], the share of address
translations resolved via the TLB; for a [[cache]], the share of data accesses found in the cache.

A high hit rate requires **good [[locality]]**.

## Context

Belongs to [[memory-management]]. It is the quantity that turns the whole chapter into
arithmetic: the **effective access time** is the hit-rate-weighted average over the levels.

## How it works

```
T = p_hit * t_hit + (1 - p_hit) * t_miss
```

applied recursively down the hierarchy.

### Worked example — TLB (p13)

Hit rate 35 %, a hit costs 10 ns, a load (miss) costs 100 ns:

```
T = 0,35 * 10 ns + 0,65 * 100 ns = 3,5 + 65 = 68,5 ns
```

### Worked example — two levels (p21)

Cache access 15 ns; not in the cache but in main memory: 45 ns to load the line into the cache;
on disk: 10 ms. Cache hit rate 80 %, main-memory hit rate 70 %.

```
T = 0,8 * 15 ns
  + 0,2 * ( 0,7 * (45 + 15) ns
          + 0,3 * (10 ms + 45 ns + 15 ns) )
  ≈ 600,025 µs
```

Note the structure: a miss at one level pays that level's cost **plus** the cost of the level
below, and the deepest term (the disk) dominates the result completely — 0,2 · 0,3 = 6 % of
accesses going to disk is enough to push the average from nanoseconds to hundreds of
microseconds.

### Page-fault rate

The lecture separates the *page-fault rate* from the TLB hit rate. It depends on:

- the **I/O time** and the memory-access characteristics — both of which favour **larger pages**
  (see [[page-size]]);
- the **[[locality|locality principle]]** — how concentrated the program's accesses are.

## Related

- [[translation-lookaside-buffer]] / [[cache]] — the levels being measured.
- [[locality]] — the precondition for a high hit rate.
- [[working-set]] — a resident working set is what produces a high hit rate.
- [[page-fault-frequency]] — using the miss rate as a control signal.

## Common confusions

- **Weight every branch by the *whole* path probability.** In the two-level example the disk
  branch carries 0,2 · 0,3, not 0,3.
- **A miss usually pays the hit cost too.** After loading the line into the cache the datum is
  still read *from the cache*, hence `(45 + 15) ns`, not `45 ns`.
- **Hit rate is not utilisation.** It says nothing about how busy the CPU is; see
  [[cpu-utilisation]].

> [!question] Unverified — the p21 figure of ≈ 600,025 µs is transcribed exactly as written in the
> notes. It is worth re-checking the unit bookkeeping of the disk term before relying on it.

## Sources

- [[lecture-bs-4]] (p12, p13, p21)
