---
title: Hot-Spare and Hot-Swap
type: concept
tags: [file-system, storage, raid, reliability]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Hot-Spare and Hot-Swap

## Definition

Two different things, distinguished by one word:

- **Hot-Spare disk** — a **reserve disk kept in the system, unused in normal operation**. When a
  disk fails, the controller immediately begins reconstructing onto it, without anyone touching
  the machine.
- **Hot-Swap disk** — the disks of a [[raid]] system are **freely accessible and can be replaced
  during running operation**, without shutting the system down.

## Why both exist

A redundant RAID level tolerates a failure, but it is **unprotected until the rebuild finishes**.
The two mechanisms attack the two halves of that window:

| | Shortens |
|---|---|
| **Hot-spare** | the time until the **rebuild starts** — from hours (waiting for a human) to seconds |
| **Hot-swap** | the time until the **array is fully staffed again** — no downtime for the replacement |

Together they mean a [[raid-5]] array can survive a disk failure with neither an outage nor a
prolonged period of vulnerability.

## Context

Belongs to [[raid]]. These are the operational counterparts of the redundancy levels: the levels
say what the array *can* survive, hot-spare and hot-swap determine how long it stays exposed.

## Related

- [[raid]] — the system they serve.
- [[raid-5]], [[raid-6]] — the rebuild window is what they shorten; RAID 6 attacks it from the
  other side, by tolerating a second failure during it.
- [[logical-volume-manager]] — "during running operation" recurs there too: enlarging and shrinking
  volumes without downtime.

## Common confusions

- **Hot-spare vs. hot-swap is a standard exam pairing** and the two are easy to blur. Mnemonic:
  a **spare** is a *disk that is already there*; a **swap** is an *action you are allowed to
  perform*. One is a component, the other a capability.

## Sources

- [[lecture-bs-5]] (p27)
