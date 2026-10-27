---
title: CPU Utilisation
type: concept
tags: [process, metrics, scheduling]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# CPU Utilisation

## Definition

**CPU utilisation** (CPU-Auslastung) is the fraction of total elapsed time during which the
processor is doing process work:

```
CPU utilisation = process time / total time
```

## Context

Belongs to [[processes]]. It is the metric that justifies nearly every mechanism in this chapter —
[[interrupt|interrupts]] instead of polling, [[direct-memory-access|DMA]] instead of per-word
copying, and multiprogramming instead of one process at a time.

## How it works

Worked example from the notes: a process runs for 6 time units and then waits for 6 time units.

```
utilisation = 6 / (6 + 6) = 0.50 = 50 %
```

The remaining 50 % is exactly the window in which the [[scheduler]] could run another process
instead — which is why a second ready process raises utilisation without making either process
faster.

## Related

- [[scheduler]] — the component that tries to maximise it.
- [[direct-memory-access]] — removes the CPU from bulk transfer, raising utilisation for useful
  work.
- [[process-state-model]] — time in *Blocked* is the time that drags utilisation down.

## Common confusions

- **Utilisation is not throughput and not speed.** A 100 %-utilised CPU may still be running
  useless work.
- The denominator is *total elapsed time*, including waiting — not just the time the process was
  in the Running state.

## Sources

- [[lecture-bs-1]] (p09)
