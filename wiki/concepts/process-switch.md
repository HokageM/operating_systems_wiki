---
title: Process Switch
type: concept
tags: [process, switching, scheduling]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-2, lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Process Switch

## Definition

A **process switch** (Prozesswechsel) is the operation by which the OS stops the currently running
[[process]] and starts a different one. Unlike a plain [[context-switch]], it really changes which
process owns the CPU.

## Context

Belongs to [[processes]]. The [[scheduler]] decides that it should happen; the [[dispatcher]]
carries it out.

## How it works

Chapter 2 breaks the cost of the switch down explicitly, as the baseline that a [[thread-switch]]
improves on: **exchange the PCB → exchange the memory-management table (Speicherverwaltungstabelle)
→ replace the memory areas → flush and reload the cache.** The eight steps below are the same
operation described procedurally.

The eight steps:

1. **Halt the process.**
2. **Switch to kernel mode** (see [[user-mode-and-kernel-mode]]).
3. **Update the PCB** of the outgoing process — save its state ([[process-control-block]]).
4. **Select the next process** (the [[scheduler]]'s decision).
5. **Prepare the MMU** for the next process (its address space).
6. **Update the PCB of the next process and load its state.**
7. **Switch to user mode.**
8. **Execute the next instruction.**

**When does a process switch happen?** The OS triggers a switch, the process ends itself, or the
process blocks. Chapter 05 adds the two triggers of a **preemptive** policy: the expiry of a
[[time-slice]] (announced by the [[clock-interrupt]]) and the arrival of a higher-priority process
in the [[ready-queue]]. See [[preemptive-and-non-preemptive-scheduling]].

**When does *no* process switch happen?** On a [[system-call]], on a switch into kernel mode, and
on an [[interrupt]] — those are only [[context-switch|context switches]].

## Related

- [[context-switch]] — the cheaper operation it is contrasted with.
- [[dispatcher]] — performs it.
- [[scheduler]] — decides on it (step 4).
- [[process-control-block]] — read and written in steps 3 and 6.
- [[process-state-model]] — a switch is a transition out of and into *Running*.
- [[thread-switch]] — the same operation between threads of one process, reduced to a single step.
- [[spin-lock]] — the cost of this operation is what decides between spinning and blocking;
  blocking on a [[mutex]] or [[semaphore]] costs **two** switches.
- [[memory-management-unit]] — step 5; the register holding the [[page-table]] start address is
  what actually gets reloaded.
- [[translation-lookaside-buffer]] — its entries belong to the old address space and are
  invalidated by the switch, which is a large part of step 5's cost.

## Common confusions

- **Process switch vs. context switch** — the single most likely exam trap from this chapter; see
  the "when does no process switch happen" list above.
- **Step 5 is what makes it expensive.** Reprogramming the MMU (and thereby invalidating cached
  address translations) is the cost of address-space isolation. Chapter 4 makes this concrete:
  what changes is the hardware register holding the [[page-table]] start address, and the price is
  a cold [[translation-lookaside-buffer|TLB]] and a cold [[cache]] afterwards.
- The switch itself is not a scheduling *strategy* — which process is picked in step 4 is a
  separate question, answered in [[scheduling]].
- **The eight-step list is examined directly.** Exercise S-1 c) of [[lecture-bs-5]] asks for it
  verbatim, and S-1 d) asks for the delta to a [[thread-switch]]: **the same list without step 5**,
  because threads share the address space and the MMU therefore needs no reprogramming. Being able
  to say *which* step disappears, and why, is the whole answer.

## Cost of preemption

Every preemption costs one process switch, so the overhead of a preemptive policy is
(number of preemptions) × (cost of these eight steps). That product is exactly why
[[round-robin]] with a small [[time-slice]] wastes the CPU, and why the quiz answer in
[[lecture-bs-5]] states that CPU utilisation *by application code* rises with growing q.

## Sources

- [[lecture-bs-1]] (p16)
- [[lecture-bs-2]] (p05) — the four-step cost breakdown vs. the thread switch
- [[lecture-bs-5]] (exercise S-1 c and d) — the eight steps confirmed verbatim; the thread switch
  is the same without step 5.
