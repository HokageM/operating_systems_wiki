---
title: Critical Section
type: concept
tags: [concurrency, synchronisation, critical-section]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Critical Section (kritischer Abschnitt)

## Definition

A **critical section** is a **section of a program in which a shared [[resource]] is accessed**,
and which therefore may be executed by **at most one process at a time**.

## Context

Belongs to [[concurrency-and-synchronisation]]. The critical section is the *unit of protection*:
[[mutual-exclusion]] is defined in terms of it, [[lock|locks]] bracket it, and [[deadlock|
deadlocks]] arise from holding one while requesting entry to another.

## How it works

The canonical shape is:

```c
lock(&l);          /* entry section  */
  /* critical section: touch the shared resource */
unlock(&l);        /* exit section   */
  /* remainder section */
```

The goal is that the bracketed region behaves like an
[[atomic-operation|atomic instruction sequence]] — indivisible as seen from other processes, even
though the hardware freely preempts inside it.

Design rules implied by the four requirements of [[mutual-exclusion]]:

- **Keep it short.** Every instruction inside it is serialised, so it is a direct limit on
  concurrency.
- **Never block inside it without releasing it.** A [[thread]] that blocks while holding the
  section prevents anyone from ever making its wait condition true — which is precisely why a
  [[condition-variable]] releases the [[mutex]] while waiting.
- **Do not put unrelated data under the same lock**, or unrelated code contends for no reason.

## Related

- [[mutual-exclusion]] — the property required of it.
- [[atomic-operation]] — the illusion it must create.
- [[lock]], [[mutex]], [[semaphore]] — what brackets it.
- [[condition-variable]] — how to wait *inside* it safely.
- [[race-condition]] — what happens without it.

## Common confusions

- **The critical section is code, not data.** Two different code regions touching the same
  variable are both critical sections *for that variable* and must share one lock.
- **Entering the critical section is not the same as holding the CPU.** The process can still be
  preempted inside it; mutual exclusion only guarantees no *other* process enters, not that this
  one runs uninterrupted.
- **A blocked thread inside the critical section is normal**, provided the [[mutex]] is released
  for the duration — see [[condition-variable]] and [[producer-consumer]].

## Sources

- [[lecture-bs-3]] (p02, p22, p25)
