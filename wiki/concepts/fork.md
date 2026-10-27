---
title: fork()
type: concept
tags: [process, creation, unix]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# fork()

## Definition

`fork()` is the system call by which **the calling process creates a copy of itself — the child
process** (Kindprozess). All processes except the initial process created at [[booting|boot]] are
created with `fork()`.

## Context

Belongs to [[processes]]. Together with [[exec]] it forms the **fork-exec mechanism**, the Unix
way of starting a new program.

## How it works

The child process:

- has its **own address space** (eigener Adressraum),
- **inherits data, code and the program counter** from the parent,
- **gets its own PID**.

The OS then runs the child independently of the parent.

**Return value** — this is what makes the single call produce two different behaviours:

| Return value | Meaning |
|---|---|
| `0` | We are in the **child** |
| `> 0` | We are in the **parent**; the value is the **child's PID** |
| `-1` | **Error** — no process was created |

The parent typically then calls `waitpid(child, &status, 0)` to wait for the child's termination
and `WEXITSTATUS(status)` to read its exit status. Without such a wait the output order of parent
and child is **non-deterministic**; with it, the order is predetermined.

Copying the whole image is expensive, especially when the child immediately calls [[exec]] — hence
[[copy-on-write]] and the historic `vfork()`.

## Related

- [[exec]] — what the child usually does next.
- [[copy-on-write]] — how the copy cost is avoided.
- [[zombie-process]] — a child the parent never collects.
- [[orphan-process]] — a child that outlives its parent.
- [[pipe]] — unnamed pipes only work between fork-related processes.
- [[pthread-api]] — `pthread_create()` is the thread-level counterpart, without the copy and
  without a kinship relation.

## Common confusions

- **`fork()` returns twice** — once in each process. This is the single most confusing thing about
  it.
- **`> 0` is the child's PID, not the parent's.** To learn its own PID a process calls `getpid()`;
  its parent's, `getppid()`.
- **The child does not share variables with the parent.** After the fork, `i` lives in a different
  address space in each process, which is why the two can print different values.
- `fork()` does not start a new *program* — that is [[exec]]'s job.

## Sources

- [[lecture-bs-1]] (p12, p18, p20)
