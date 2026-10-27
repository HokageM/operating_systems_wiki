---
title: Cooperative Scheduling
type: concept
tags: [process, scheduling]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Cooperative Scheduling

## Definition

Scheduling in which a process **voluntarily yields the processor** (freiwillige Abgabe des
Prozessors). The notes introduce it as the named special case of
[[preemptive-and-non-preemptive-scheduling|non-preemptive]] scheduling: the CPU is never taken, but
it may be given back.

## Context

Belongs to [[scheduling]]. It is what remains of multiprogramming if you remove the
[[clock-interrupt]]: the system still switches between processes, but only at points the processes
themselves choose.

## How it works

The running process calls a yield operation (or simply blocks on I/O), which enters the kernel,
runs the [[scheduler]], and lets the [[dispatcher]] hand the CPU to someone else. Control returns
to the yielding process when it is selected again.

## Related

- [[preemptive-and-non-preemptive-scheduling]] — the classification it belongs to.
- [[process-state-model]] — a voluntary yield is a **Running → Ready** transition performed by the
  process itself.
- [[spin-lock]] — the counter-example: a process that busy-waits without yielding blocks the whole
  system under cooperative scheduling, which is why spin locks are only viable with preemption or
  on multiprocessors.

## Common confusions

- **Cooperative ≠ fair.** Nothing forces a process to yield. A single non-cooperating process
  freezes the machine — the reason cooperative scheduling was abandoned for general-purpose
  systems.
- It is still used where processes are trusted and switching must be cheap: coroutines,
  event loops, user-level thread libraries.

## Sources

- [[lecture-bs-5]] (p01)
