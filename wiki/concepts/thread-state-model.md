---
title: Thread State Model
type: concept
tags: [thread, states, scheduling]
chapter: "02"
sources: [lecture-bs-2]
created: 2026-09-30
updated: 2026-09-30
---

# Thread State Model

## Definition

The **thread state model** describes the four state transitions a [[thread]] goes through:
**creation (Erzeugung)**, **blocking (Blockierung)**, **unblocking (Aufhebung der Blockierung)**
and **termination (Beendigung)**.

## Context

Belongs to [[threads]]. It mirrors the [[process-state-model]] one level down, but the notes give
it as four *transitions* rather than as a numbered state diagram.

## How it works

1. **Erzeugung** — when a new [[process]] is created, its **main thread (Haupt-Thread)** is
   created with it and inserted into the **ready queue (Bereit-Queue)**. The main thread can then
   create further threads ([[pthread-api|`pthread_create()`]]).
2. **Blockierung** — if a thread waits for an event, it is blocked.
3. **Aufhebung der Blockierung** — when the event on whose account the thread was blocked occurs,
   the thread is moved back into the **ready queue**.
4. **Beendigung** — the resources it occupied are released.

### Termination conditions

The exercise recap asks *"Wann wird ein Prozess bzw. Thread beendet?"*:

| Process end (Prozessende) | Thread end (Threadende) |
|---|---|
| the `main` function is left | the thread function is left |
| `exit()` is called | `pthread_exit()` is called |
| the last thread terminates itself | the process is terminated |

The two columns are linked in both directions: killing the process kills every thread, and the
death of the last thread ends the process.

## Related

- [[process-state-model]] — the same ready/blocked/running logic at process level.
- [[thread]] — the subject.
- [[pthread-api]] — `pthread_create`, `pthread_exit`, `pthread_join`.
- [[scheduler]] — what the ready queue feeds.

## Common confusions

- **"Ready queue" is shared vocabulary with processes.** Threads are queued for the CPU in the
  same way; the states are not a separate mechanism, just a finer granularity.
- **Termination is asymmetric.** A thread ending does not end the process (unless it is the last
  one), but the process ending always ends every thread — immediately, with no chance to finish.
  This is what happens when `pthread_join()` is omitted.

## Sources

- [[lecture-bs-2]] (p02, p08)
