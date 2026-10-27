---
title: Conflict
type: concept
tags: [concurrency, synchronisation, conflict]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Conflict (Konflikt)

## Definition

A **conflict** arises when **two or more processes access the same
[[resource|resource]] concurrently** and at least one of them modifies it.

This is the boxed definition that opens chapter 3, and it is asked verbatim in the exercise recap
(3-1 a).

## Context

Belongs to [[concurrency-and-synchronisation]]. A conflict is the *precondition* for trouble, not
the trouble itself: concurrent **reads** of the same variable conflict in the general sense but
cause no harm. What makes a conflict dangerous is when the outcome depends on the interleaving —
then it is a [[race-condition]].

## How it works

Conflicts become possible as soon as two strands of execution share state:

- [[thread|Threads]] of one [[process]] share the whole data segment by construction, so *every*
  global variable is a potential conflict.
- Processes share state only deliberately — through shared memory, files, or devices — so the
  conflict surface is smaller (see [[interprocess-communication]]).

The classic example is `x = x + 1` executed by two strands. The statement compiles into read,
increment, write; a [[process-switch]] between the read and the write of one strand loses the
other's update entirely.

## Related

- [[race-condition]] — a conflict whose outcome depends on timing.
- [[critical-section]] — the code region in which the conflicting access happens.
- [[mutual-exclusion]] — the remedy.
- [[resource]] — what is being contended for.
- [[thread]] — why conflicts are the normal case inside a process.

## Common confusions

- **Conflict ≠ race condition.** A conflict is "they touch the same thing"; a race condition is
  "and the result depends on who gets there first". Every race condition is a conflict; not every
  conflict is a race condition.
- **Conflicts are not solved by making code shorter.** Preemption can occur between any two
  machine instructions — even a single high-level statement is not safe. Only
  [[atomic-operation|atomicity]] helps.

## Sources

- [[lecture-bs-3]] (p01, p22)
