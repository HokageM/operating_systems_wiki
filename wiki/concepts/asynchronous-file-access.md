---
title: Asynchronous File Access
type: concept
tags: [file-system, storage, io]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Asynchronous File Access

## Definition

**Non-blocking reading and writing** of a [[file]]: the call returns immediately, before the data
has actually been transferred, and the process continues running. It is notified later that the
operation has completed.

## Context

Belongs to [[file-systems]]. Under ordinary synchronous access a `read()` puts the process into
state **Blocked** (see [[process-state-model]]) for the whole duration of the transfer — which,
per [[disk-access-time]], is around 10 ms, or tens of millions of instructions the CPU could have
executed instead.

## How it works

The process issues the request and keeps running. The transfer proceeds in the background, usually
by [[direct-memory-access]], and completion is announced by an [[interrupt]] which the operating
system turns into a notification — in UNIX typically a [[signal]], handled by a
[[signal-handler]].

## Related

- [[direct-memory-access]] — the hardware that makes the background transfer free of CPU cost.
- [[process-state-model]] — asynchronous access avoids the Running → Blocked transition.
- [[signal]] and [[signal-handler]] — the completion notification path.
- [[threads]] — the alternative way to get the same effect: let one thread block synchronously
  while the others continue. Cheaper to program, more expensive to run.
- [[page-locking]] — the buffer of an in-flight asynchronous transfer must not be paged out, which
  is exactly one of the two reasons page locking exists.

## Common confusions

- **Asynchronous is not faster per operation.** The transfer takes just as long; what changes is
  that the process does not sit idle during it. The gain is in overlap, not in latency.
- **Asynchronous access reintroduces a synchronisation problem.** The buffer must not be touched
  until the completion notification arrives — a [[race-condition]] waiting to happen.

## Sources

- [[lecture-bs-5]] (p13)
