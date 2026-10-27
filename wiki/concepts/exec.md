---
title: exec()
type: concept
tags: [process, creation, unix]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# exec()

## Definition

`exec()` **overlays the running program with a new one and begins executing the new program**
(das laufende Programm wird mit dem angegebenen Programm überlagert). Its argument is the **path
name of the program to load**; the **environment (Umgebung) of the process is preserved**.

**If `exec()` succeeds it does not return.** On error it returns `-1`.

## Context

Belongs to [[processes]]. Together with [[fork]] it forms the **fork-exec mechanism**: fork to get
a new process, exec to give it a different program.

## How it works

The process keeps its identity — same PID, same open files, same environment — but its program,
data, heap and stack ([[process-image]]) are replaced.

The C variant noted in the source:

```c
int execlp(const char *file, const char *arg, ...);
```

The running program is overlaid with the specified program, which is then executed with the passed
parameters. With the `p` variants the program is searched for along **`PATH`** — a list of
directories in which programs are searched for.

A shell does exactly this for every command: [[fork]] a copy of itself, then `exec` the requested
program in the child.

## Related

- [[fork]] — creates the process that exec then repurposes.
- [[copy-on-write]] — makes the fork-then-exec pattern cheap.
- [[process-image]] — what exec replaces.
- [[pipe]] — `dup2()` redirections are set up between fork and exec.

## Common confusions

- **`exec()` does not create a process.** The PID is unchanged; there is no parent/child
  relationship involved.
- **"Returns only on failure"** — code written after a successful `exec` call never runs. This is
  a favourite exam observation.
- The *environment* survives, but the program and its data do not.

## Sources

- [[lecture-bs-1]] (p13, p18, p23)
