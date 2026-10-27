---
title: Pipe
type: concept
tags: [ipc, unix, io]
chapter: "01"
sources: [lecture-bs-1]
created: 2026-09-30
updated: 2026-09-30
---

# Pipe

## Definition

A **pipe** is a **ring buffer** (Ringpuffer) that **supports the communication of two processes**.
One process writes into it, the other reads from it.

## Context

The main mechanism of [[interprocess-communication]] in this chapter, alongside the [[signal]].

## How it works

The lecture's sketch: a parent calls [[fork]] twice, producing children A and B; A calls `write()`
into the pipe and B calls `read()` from it.

```c
int pipefd[2];
pipe(pipefd);        // creates an unnamed pipe
// child (writer):
dup2(pipefd[1], STDOUT_FILENO);
// parent (reader):
dup2(pipefd[0], STDIN_FILENO);
```

`pipefd[0]` is the **read** end, `pipefd[1]` the **write** end. `dup2()` redirects a standard
stream onto a descriptor, which is how a pipe becomes transparent to a program that only knows
about stdin/stdout.

The same `dup2()` trick redirects to files — e.g. suppressing stdout:

```c
fd = fopen(...);
dup2(fileno(fd), STDOUT_FILENO);  // everything written to stdout now goes to fd
```

**In the shell:**

| Command | Meaning |
|---|---|
| `ls > output.txt` | `ls` is executed and its output is passed to the file `output.txt` |
| `wc -l < output.txt` | `wc -l` receives the file as its standard input |
| `ls \| wc -l` | the output of `ls` is piped to `wc -l` as its input |

## Related

- [[signal]] — the other IPC mechanism; carries no data.
- [[fork]] — an *unnamed* pipe can only be shared between related processes.
- [[exec]] — the redirections are set up after fork and survive exec.
- [[thread]] — threads of one process need no pipe at all; they share the data segment.
- [[producer-consumer]] — a pipe is the OS-level instance of the bounded-buffer pattern: a full
  pipe blocks the writer, an empty pipe blocks the reader.
- [[consumable-and-reusable-resources]] — the bytes in a pipe are a consumable resource.

## Common confusions

- **A pipe is unidirectional.** Two pipes are needed for a bidirectional dialogue.
- **`pipefd[0]` read, `pipefd[1]` write** — mixing the indices up is the classic mistake; mnemonic:
  0 like stdin, 1 like stdout.
- **`>` is not a pipe.** `>` and `<` redirect to/from a *file*; `|` connects two *processes*.
- The buffer is finite: a writer blocks when the ring buffer is full, a reader when it is empty —
  which is why a pipe also provides synchronisation, not just data transfer.

## Sources

- [[lecture-bs-1]] (p13, p22, p23)
