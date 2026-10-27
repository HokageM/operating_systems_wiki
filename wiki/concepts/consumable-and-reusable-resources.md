---
title: Consumable and Reusable Resources
type: concept
tags: [deadlock, resources, taxonomy]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Consumable and Reusable Resources (konsumierbare und wiederverwendbare Betriebsmittel)

## Definition

Chapter 3 splits [[resource|resources]] into two kinds for the purposes of deadlock analysis:

- A **reusable resource** (wiederverwendbares Betriebsmittel) is **not used up**: it is allocated
  to one process at a time, used, and **returned unchanged**. Examples: the CPU, main memory,
  files, devices, [[semaphore|semaphores]], [[mutex|mutexes]].
- A **consumable resource** (konsumierbares Betriebsmittel) is **created and destroyed**: it is
  produced by one process, and consuming it removes it. Examples: messages, [[signal|signals]],
  interrupts, items in a buffer.

## Context

Belongs to [[deadlocks]]. The distinction matters because the deadlock *models* differ: the
[[resource-allocation-graph]] and the [[deadlock-detection-marking-algorithm]] both assume
**reusable** resources with a fixed total count.

## How it works

| | **Reusable** | **Consumable** |
|---|---|---|
| Total count | fixed — the resource vector **R** | unbounded, created at runtime |
| Allocation | granted and later released | produced and consumed |
| Deadlock shape | circular wait for held resources | everyone waits for a message nobody will send |
| Modelled by | [[resource-allocation-graph]], marking algorithm | not modelled in this chapter |

For reusable resources the chapter also distinguishes **single-instance** from
**multi-instance**:

- **Single instance** (one printer, one mutex): a **cycle** in the
  [[resource-allocation-graph]] is **necessary and sufficient** for a deadlock.
- **Multiple instances** (three identical tape drives): a cycle is only **necessary**; deciding
  deadlock requires the [[deadlock-detection-marking-algorithm]] over R, V, A and Z.

This single/multi split is the reason the chapter presents two detection procedures rather than
one.

## Related

- [[resource]] — the general notion from chapter 1.
- [[deadlock]] — what the taxonomy is for.
- [[resource-allocation-graph]] — the single-instance model.
- [[deadlock-detection-marking-algorithm]] — the multi-instance model.
- [[semaphore]] — a counting semaphore models *n* instances of a reusable resource.
- [[producer-consumer]] — buffer items are the standard consumable resource.

## Common confusions

- **A consumable resource can deadlock too**, even though the chapter does not model it: if every
  process in a set blocks waiting for a message that only another member would send, the deadlock
  definition is satisfied exactly.
- **"Reusable" does not mean "shareable".** A reusable resource is still exclusive while held —
  reuse happens *after* release.
- **A counting [[semaphore]] is a reusable resource with n instances**, not a consumable one, even
  though its counter goes up and down.

## Sources

- [[lecture-bs-3]] (p13, p18)
