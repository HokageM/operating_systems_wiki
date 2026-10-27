---
title: Copy-on-Write
type: concept
tags: [process, memory, optimisation]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Copy-on-Write

## Definition

**Copy-on-write (COW)** is the optimisation in which, after [[fork]], **parent and child share the
same memory — no copying takes place. Only on a modification (Änderung) is the corresponding
memory segment copied.** Therefore only the [[process-control-block|PCB]] and a little memory are
actually copied at process creation.

## Context

Belongs to [[processes]]. It is the answer to the exercise question: *the shell creates a full copy
of itself on every program start and then immediately overlays it with [[exec]] — how can the OS
avoid this waste?*

## How it works

The child's pages are mapped to the parent's physical pages, marked read-only. The first write by
either side faults, and only then is that one segment duplicated and made writable. If the child
calls [[exec]] before writing anything — the overwhelmingly common case for a shell — almost
nothing is ever copied.

### Alternative: `vfork()`

`vfork()` creates a new process and **suspends the parent until the child terminates or calls
`exec()`**. Until then **the child must not modify any data**. This achieves the same saving by
restricting the child instead of by hardware support.

| | **`vfork()`** | **Copy-on-write** |
|---|---|---|
| Parent | Suspended until child execs or exits | Runs normally |
| Child | Must not modify data | May do anything |
| Copying | None | Deferred, per segment, on first write |
| Requires | Nothing special | MMU support / page protection |

## Related

- [[fork]] — what is being optimised.
- [[exec]] — the call that makes the copy pointless.
- [[process-image]] — what would otherwise be duplicated.
- [[locality]] — most forked children touch very little memory.
- [[paging]] — COW works at page granularity; "mark read-only" means clearing the Write bit in
  the **Prot** field of the [[page-table-entry]].
- [[page-fault]] — the write to a read-only shared page is what triggers the actual copy.
- [[page-locking]] — shared frames reachable from several page tables are handled specially by
  [[page-replacement]].
- [[demand-paging]] — the same "do no work until it is provably needed" principle.

## Common confusions

- **COW is not lazy loading.** The data is already there; what is deferred is the *duplication*.
- **The PCB is always copied** — COW defers the user memory, not the control structures.
- `vfork()`'s restriction is real: a child that writes data before `exec` corrupts the parent.
- **The COW fault is a protection fault, not a [[page-fault]] in the Present/Absent sense.** The
  page *is* resident; the access simply violates the protection bits. Both trap into the OS, but
  the handling differs.

## Sources

- [[lecture-bs-1]] (p21)
- [[lecture-bs-4]] (p10) — the protection bits COW relies on
