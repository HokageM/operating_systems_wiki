---
title: Livelock
type: concept
tags: [deadlock, livelock, concurrency]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Livelock

## Definition

A **livelock** is a state of a set of processes in which the processes **continuously change
their state but make no progress** — they wait **actively** instead of passively.

## Context

Belongs to [[deadlocks]]. The livelock is the active twin of the [[deadlock]] and is best defined
by contrast with it.

## How it works

| | [[deadlock]] | **Livelock** |
|---|---|---|
| Waiting is | **passive** | **active** |
| [[process-state-model]] state | **Blocked** | cycles between **Ready** and **Running** |
| CPU consumed | none | full |
| Visible symptom | processes frozen, CPU idle | processes busy, CPU at 100 %, nothing happens |

The classic generator is **symmetric retreat**: each process notices it might deadlock, releases
what it holds, waits, and retries — and because all of them do exactly the same thing at the same
time, they collide again. The [[dining-philosophers]] problem produces this directly: if every
philosopher picks up the left fork, sees that the right one is taken, puts the left one down and
retries, they loop forever without ever eating.

Livelock is in one sense worse than deadlock: it is harder to detect (no process is blocked, so
no queue looks suspicious) and it consumes the CPU while failing.

## Related

- [[deadlock]] — the passive counterpart.
- [[starvation]] — a single process losing, rather than a set spinning.
- [[dining-philosophers]] — where both failure modes appear.
- [[spin-lock]] — also active waiting, but *not* a livelock: a spinner does proceed eventually.

## Common confusions

- **Busy waiting is not a livelock.** A [[spin-lock]] spins on a condition that *will* become
  true. A livelock spins on one that will not.
- **A livelock is not a slow deadlock.** In principle it can resolve itself if the symmetry is
  broken (e.g. by randomised back-off); a deadlock never can.
- **Detection tools that look for blocked processes miss livelocks entirely**, because nothing is
  blocked.

## Sources

- [[lecture-bs-3]] (p14, p15, p16)
