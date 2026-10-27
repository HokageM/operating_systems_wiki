---
title: Clock Interrupt
type: concept
tags: [process, scheduling, interrupts]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Clock Interrupt

## Definition

The periodic [[interrupt]] raised by a hardware timer. It is the mechanism by which a
**[[time-slice]] is realised**, and therefore the hardware foundation of all preemptive,
time-slice based [[scheduling]].

## Context

Belongs to [[interrupts-and-io]] and to [[scheduling]]. Chapter 01 defined what an interrupt is;
this chapter names the one interrupt source that exists purely for the operating system's own
purposes — no device requests anything, the timer simply gives the kernel a guaranteed opportunity
to regain control.

## How it works

1. The timer is programmed to fire at a fixed rate.
2. Each tick enters the kernel through the [[interrupt-vector-table]] and runs an
   [[interrupt-service-routine]].
3. The handler decrements the running process's remaining quantum.
4. At zero it invokes the [[scheduler]], and the [[dispatcher]] performs a [[process-switch]].

Because the timer is independent of the running program, **the kernel can always take the CPU
back**. Without it only [[cooperative-scheduling]] is possible.

## Related

- [[interrupt]] — the general mechanism.
- [[time-slice]] — what the clock interrupt counts down.
- [[round-robin]] — the algorithm it implements.
- [[preemptive-and-non-preemptive-scheduling]] — one of the two triggers of preemption.
- [[context-switch]] — every tick costs at least a context switch, even when no process switch
  follows.

## Common confusions

- **Not every clock interrupt causes a process switch.** Most ticks only decrement a counter and
  return to the same process. The switch happens when the quantum reaches zero.
- The clock interrupt is **asynchronous**, unlike a [[trap]]: it is not caused by the running
  instruction and is neither predictable nor reproducible from the program's point of view.

## Sources

- [[lecture-bs-5]] (p02, p06)
