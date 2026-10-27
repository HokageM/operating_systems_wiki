---
title: Swapping
type: concept
tags: [process, memory, states]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Swapping

## Definition

**Swapping** (Auslagern) is **moving the data of a blocked [[process]] out to persistent
storage**. It frees main memory for other processes.

## Context

Belongs to [[processes]]. Swapping is what forces the [[process-state-model]] from five states to
seven, by adding *Ready/Suspend* and *Blocked/Suspend*.

## How it works

Reasons for swapping, per the lecture:

- release main memory for another process;
- a process causing problems is suspended;
- user request (Benutzerwunsch).

A suspended process is not in memory, so it cannot be dispatched even when it becomes ready. The
transitions are *Suspend* (out) and *Activate* (back in). Note that
**Blocked/Suspend --Event Occurs--> Ready/Suspend**: the awaited event may arrive while the
process is still swapped out.

Swapping pays off because of [[locality]] — the process least recently active is the one least
likely to be needed next.

### Two granularities

Chapter 4 revisits the same idea at a finer grain. Under [[virtual-memory]] it is **individual
pages**, not whole processes, that move between main memory and the swap area, driven by a
[[page-fault]] rather than by a scheduling decision. The two coexist:

| | Whole-process swapping (ch.01) | Page-level swapping (ch.04) |
|---|---|---|
| Unit | the whole [[process-image]] | one [[page-and-frame|page]] |
| Trigger | OS decision: memory pressure, problem process, user request | a [[page-fault]] |
| Visible in | the 7-state [[process-state-model]] | nowhere — it is transparent |
| Purpose | free a lot of memory at once | let a process run without being fully resident |

Swapping a whole process out is also the standard remedy against [[thrashing]]: reducing the
degree of multiprogramming gives the remaining processes a complete [[working-set]] each.

## Related

- [[process-state-model]] — the 7-state variant exists because of swapping.
- [[locality]] — the assumption behind eviction decisions.
- [[process-image]] — what is actually written out.
- [[virtual-memory]] — the same mechanism at page granularity.
- [[page-replacement]] — how the victim is chosen when the unit is a page.
- [[thrashing]] — the failure mode that whole-process swapping is used to cure.

## Common confusions

- **Suspended ≠ blocked.** Blocked means waiting for an event; suspended means not in main memory.
  A process can be both (Blocked/Suspend).
- **Swapping (ch.01) ≠ paging out (ch.04).** Both write memory contents to the swap area, but the
  unit and the trigger differ — see the table above. Exam answers should name the granularity.
- **Suspension is not only for blocked processes** — the lecture also allows
  Running --Suspend--> Ready/Suspend and admitting a New process directly to Ready/Suspend.

## Sources

- [[lecture-bs-1]] (p11)
