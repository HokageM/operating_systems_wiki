---
title: Atomic Operation
type: concept
tags: [concurrency, synchronisation, atomicity, hardware]
chapter: "03"
sources: [lecture-bs-3, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Atomic Operation (atomare Anweisungsfolge)

## Definition

An **atomic instruction sequence** (atomare Anweisungsfolge) is a sequence of instructions that
is **either executed completely or not at all**, and which **cannot be interrupted** by other
processes — no intermediate state of it is ever observable from outside.

## Context

Belongs to [[concurrency-and-synchronisation]]. Atomicity is the property that a
[[critical-section]] must simulate and that [[mutual-exclusion]] supplies. It is also what the
hardware gives directly in the case of [[test-and-set]].

## How it works

Atomicity comes from one of three places:

1. **The hardware.** A single machine instruction is atomic with respect to preemption. Special
   read-modify-write instructions such as [[test-and-set]] extend this to a test *and* a store,
   which is exactly what a naive lock variable lacks.
2. **Disabling [[interrupt|interrupts]].** With no interrupts there is no preemption, so any
   sequence is atomic — on a single CPU only, and only in
   [[user-mode-and-kernel-mode|kernel mode]].
3. **A synchronisation primitive.** A [[lock]], [[mutex]] or [[semaphore]] makes the bracketed
   region *behave* atomically toward other participants, even though it is really interruptible.

The notes stress that `acquire()` and `release()` of a [[semaphore]] must themselves be atomic —
which is why they are realised as [[system-call|system calls]].

## Related

- [[critical-section]] — the region that is made to appear atomic.
- [[test-and-set]] — atomicity delivered by one instruction.
- [[mutual-exclusion]] — the general mechanism.
- [[race-condition]] — what non-atomicity causes.
- [[interrupt]] — what atomicity must exclude.
- [[raid-consistent-update-problem]] — a logical RAID write must be atomic across several disks; the write-ahead log manufactures that.
- [[journaling-file-system]] — the same construction one layer up, for a multi-block file-system update.

## Common confusions

- **A single line of source is not atomic.** `x++`, `count = count + 1` and `if (locked) locked =
  true;` all compile to several instructions and can all be split by preemption. This is the whole
  reason chapter 3 exists.
- **Atomicity is relative to observers.** The sequence *is* interrupted by the CPU in practice;
  what matters is that no other participant can observe a partial state.
- **Atomic ≠ ordered.** Atomicity says nothing about *when* an operation happens relative to
  others, only that it is indivisible. Fairness is a separate requirement.

## Sources

- [[lecture-bs-3]] (p02, p05, p08)
- [[lecture-bs-5]] (p19, p23) — atomicity built from a log, twice, at two different layers.
