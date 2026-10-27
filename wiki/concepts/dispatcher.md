---
title: Dispatcher
type: concept
tags: [process, scheduling]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Dispatcher

## Definition

The **dispatcher** is the program which **realises the switching between processes** (welches die
Umschaltung zwischen Prozessen realisiert).

## Context

Belongs to [[processes]]. It is the *executor*; the [[scheduler]] is the *decider*.

## How it works

Once the [[scheduler]] has selected the next process, the dispatcher performs the
[[process-switch]]: saving the outgoing process's state into its [[process-control-block]],
preparing the MMU, loading the incoming process's state, switching back to user mode and resuming
execution. In the [[process-state-model]] it performs the **Ready → Running** transition (the
"Dispatch" arrow).

## Related

- [[scheduler]] — decides *when* and *to whom*.
- [[scheduling]] — the policies the scheduler applies; the dispatcher is policy-free.
- [[time-slice]] — when a slice expires the scheduler is asked, then the dispatcher acts.
- [[process-switch]] — the eight steps the dispatcher executes.
- [[process-state-model]] — the "Dispatch" transition.
- [[thread-switch]] — the same job between threads, at a fraction of the cost.

## Common confusions

- **Dispatcher vs. scheduler** — asked verbatim in the exercise recap. Mnemonic: the *scheduler
  chooses*, the *dispatcher moves*. Answering only "both switch processes" scores nothing.
- The dispatcher implements no policy; it contains no priorities or time slices.

## Sources

- [[lecture-bs-1]] (p09, p19)
- [[lecture-bs-5]] (p01) — the definition is repeated verbatim at the head of the scheduling
  chapter, which confirms how reliably it is examined.
