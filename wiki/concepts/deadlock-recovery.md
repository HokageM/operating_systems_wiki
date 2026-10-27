---
title: Deadlock Recovery
type: concept
tags: [deadlock, recovery, strategy]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Deadlock Recovery (Deadlock-Behebung)

## Definition

**Deadlock recovery** is the set of measures taken **after** a [[deadlock]] has been found by
[[deadlock-detection]], in order to break the circular wait and let the system continue.

## Context

Belongs to [[deadlocks]]. Recovery is the mandatory second half of the detect-and-recover
strategy: detection that is not followed by recovery leaves the system exactly as stuck as before.

## How it works

The notes give three measures, in increasing gentleness and increasing cost:

| Measure | German | How it works | Cost |
|---|---|---|---|
| **Abort all deadlocked processes** | Prozessabbruch | kill every process in the deadlocked set at once | fastest; **all** their work is lost |
| **Successive abort** | sukzessiver Abbruch | kill **one** process, release its resources, **re-run detection**; repeat until the deadlock is gone | loses the least work, but detection must run again after every kill |
| **Checkpointing and rollback** | Rücksetzpunkte / Rollback | periodically save process state; on deadlock, roll a process back to its last checkpoint and restart it from there | least work lost, but checkpoints cost time and storage **even when nothing goes wrong** |

All three rely on **preempting** a resource — i.e. on breaking Coffman condition 3 after the
fact, by force rather than by design (which is what [[deadlock-prevention]] does in advance).

The unsolved part is **victim selection**: which process to abort or roll back. Candidates would
be the process with the least work done, the fewest resources held, or the lowest priority — the
notes do not prescribe a rule.

## Related

- [[deadlock-detection]] — what must run first.
- [[deadlock]] — what is being broken.
- [[deadlock-prevention]] — the same condition (no preemption) attacked in advance instead.
- [[process]] — aborting one means terminating it and freeing its
  [[process-control-block|PCB]] and resources.

## Common confusions

- **Rollback is not free even in the good case.** Checkpointing is continuous overhead; abort is
  zero overhead until a deadlock actually occurs. This is the real trade-off.
- **Killing one process may not be enough.** That is exactly why successive abort re-runs
  detection after each kill.
- **Rolling back a process does not undo its external effects** — output already written, messages
  already sent. Only internal state is restored.

## Sources

- [[lecture-bs-3]] (p20)
