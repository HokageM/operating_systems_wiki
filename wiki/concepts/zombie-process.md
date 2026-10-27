---
title: Zombie Process
type: concept
tags: [process, termination, unix]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Zombie Process

## Definition

A **zombie process** (Zombie-Prozess) is a process that **no longer exists, but leaves a record
for its parent process to collect**. It arises when a child process ends before the parent has
collected its exit status.

Its state in the process table is shown as **`Z`**. The lecture explicitly notes that this is the
**normal situation** — every terminating child is briefly a zombie.

## Context

Belongs to [[processes]]. It is one of the two death anomalies of the [[fork]] model, the other
being the [[orphan-process]].

## How it works

When a child terminates, the OS cannot discard its [[process-control-block]] entirely: the exit
status still has to be delivered. The entry is kept until the parent calls `waitpid()`:

```c
int status;
waitpid(childID, &status, 0);
WEXITSTATUS(status);   // get exit status from child
```

Once collected, the entry disappears. A parent that never waits leaves zombies accumulating in the
process table — that is the pathological case, not the brief normal one.

## Related

- [[orphan-process]] — the mirror-image anomaly (parent dies first).
- [[fork]] — creates the parent/child relationship.
- [[process-control-block]] — what is left behind.

## Common confusions

- **A zombie consumes no CPU and no memory** beyond its process-table entry — it is not "still
  running".
- **Zombie ≠ orphan.** Zombie: *child* ended, parent has not reaped it. Orphan: *parent* ended
  first. See [[orphan-process]].
- **Being a zombie is normal and transient**; the problem is only a parent that never reaps.

## Sources

- [[lecture-bs-1]] (p18, p23)
