---
title: Orphan Process
type: concept
tags: [process, termination, unix]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Orphan Process

## Definition

An **orphan process** (Waisenprozess) is a process whose **parent has terminated before it**. The
orphan is re-parented, so that its **PPID becomes 1** (the initial process created at
[[booting|boot]]).

## Context

Belongs to [[processes]]. It is the mirror image of the [[zombie-process]].

## How it works

Exercise case from the notes: the child waits on an input while the parent terminates first — an
orphan arises and is adopted by the init process. Adoption is necessary so that someone is still
there to collect the child's exit status when it eventually ends, avoiding a permanent
[[zombie-process]].

A process can test for this itself:

```c
if (getppid() == 1) { /* orphan */ }
```

## Related

- [[zombie-process]] — child ends first instead.
- [[fork]] — establishes the parent/child link that is being broken.
- [[booting]] — the initial process (PID 1) that adopts orphans.

## Common confusions

- **Orphan ≠ zombie.** Orphan: parent died first, the child *is still running*. Zombie: child died
  first and has not been reaped.
- **An orphan is not killed.** It keeps running normally; only its PPID changes.
- `getppid() == 1` is a *heuristic test*, valid because PID 1 is the init process.

## Sources

- [[lecture-bs-1]] (p20, p23)
