---
title: Thrashing
type: concept
tags: [memory, virtual-memory, performance]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Thrashing

## Definition

**Thrashing** (Seitenflattern) is the state in which a page that has **just been displaced is
needed again immediately**, so the system executes only **a few instructions per page load**.
Almost all of the CPU's time goes into resolving [[page-fault|page faults]] rather than doing
useful work.

## Context

Belongs to [[virtual-memory]]. It is the failure mode of memory over-commitment, and the direct
consequence of main memory not being able to hold the [[working-set|working sets]] of the running
processes.

## How it works

The vicious circle:

1. Too little memory per process → the working set does not fit.
2. A page is evicted that is still in active use.
3. The next reference to it faults → a disk access of milliseconds.
4. To load it, another still-needed page must be evicted.
5. Back to step 2.

CPU utilisation collapses. Naively adding *more processes* (because the CPU looks idle) makes it
strictly worse, since the available frames are divided even further.

### Remedies

- Give the process **more frames** — [[page-fault-frequency]] does this automatically.
- **Reduce the degree of multiprogramming** — swap a whole process out ([[swapping]]) so the
  remaining ones get a complete working set each.
- Choose a better [[page-replacement]] victim — a good algorithm delays thrashing but cannot
  prevent it if memory is genuinely too small.

## Related

- [[working-set]] — thrashing is what happens when it does not fit.
- [[page-fault]] — the event whose rate explodes.
- [[page-fault-frequency]] — the control mechanism designed to avoid it.
- [[livelock]] — a structurally similar failure: the system is busy but makes no progress.

## Common confusions

- **Thrashing is not caused by a bad replacement algorithm.** Even an optimal algorithm thrashes
  if memory is too small. The cause is *insufficient frames*, not *wrong choices*.
- **A high page-fault rate alone is not thrashing.** Process start-up under [[demand-paging]]
  produces a burst of faults and then settles. Thrashing is the *sustained* state.

## Sources

- [[lecture-bs-4]] (p13)
