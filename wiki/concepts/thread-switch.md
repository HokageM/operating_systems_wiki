---
title: Thread Switch
type: concept
tags: [thread, switching, scheduling, performance]
chapter: "02"
sources: [lecture-bs-2, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Thread Switch

## Definition

A **thread switch** (Threadwechsel) is the operation of stopping one [[thread]] and running
another thread **of the same [[process]]**. It consists of **exchanging the
[[thread-control-block|TCB]]** — and nothing else.

## Context

Belongs to [[threads]]. It is the cheap sibling of the [[process-switch]], and the direct answer
to the exam question *"Was passiert bei einer Umschaltung zwischen Threads im Gegensatz zu einer
Umschaltung zwischen Prozessen?"*.

## How it works

| [[process-switch]] (Prozesswechsel) | Thread switch (Threadwechsel) |
|---|---|
| exchange the [[process-control-block\|PCB]] | exchange the [[thread-control-block\|TCB]] |
| exchange the memory-management table (Speicherverwaltungstabelle) | — |
| replace the memory areas (Speicherbereiche ersetzen) | — |
| flush and reload the cache (Entleeren & Neuladen des Cache) | — |

**⇒ the thread switch is much more efficient** (viel effizienter).

The three missing rows are all consequences of the address space staying the same: no MMU
reprogramming, no address-space swap, and — critically — no cache invalidation, so the working set
stays warm across the switch. This is the same cost structure that makes a plain
[[context-switch]] cheaper than a process switch, applied to scheduling.

## Related

- [[process-switch]] — the 8-step expensive version, and the thing it is compared against.
- [[context-switch]] — the interrupt-level switch, likewise cheap for likewise reasons.
- [[thread-control-block]] — the payload of the switch.
- [[locality]] — why keeping the cache warm matters so much.
- [[dispatcher]] — performs the switch.
- [[process-switch]] — the eight steps; a thread switch is the same list **without step 5**.
- [[scheduling]] — the strategies apply to threads just as they do to processes.

## Common confusions

- **Only threads of the *same* process switch cheaply.** Switching between threads of two
  different processes is a process switch with extra steps — the memory tables and cache must
  still change.
- **"Thread switch = TCB exchange" is the whole answer.** The exam wants the contrast table, not a
  narrative; the marks are in naming the three things the process switch does *additionally*.
- Do not confuse this with [[simultaneous-multithreading]], which avoids switching altogether by
  duplicating the register sets in hardware.

## Sources

- [[lecture-bs-2]] (p01, p05)
- [[lecture-bs-5]] (exercise S-1 d) — the thread switch is the process switch without the MMU step.
