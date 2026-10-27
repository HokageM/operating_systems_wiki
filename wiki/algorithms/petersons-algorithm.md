---
title: Peterson's Algorithm
type: algorithm
tags: [concurrency, mutual-exclusion, algorithm, software-solution]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Peterson's Algorithm

## Purpose

Achieve [[mutual-exclusion]] between **two** processes using **software only** — no special
hardware instruction, no disabling of [[interrupt|interrupts]] — while satisfying all four
requirements: correctness, progress, fairness and freedom from [[starvation]].

It is the answer to the failure of the two naive attempts: the plain lock variable (not
[[atomic-operation|atomic]]) and strict alternation (violates progress). See [[lock]] for both.

## Idea

Combine the two broken ideas so that each fixes the other's flaw:

- An **interest flag** per process (`taken[i]`) — "I want to enter". This is what secures
  **correctness**.
- A **turn variable** (`turn`) — "if we both want in, the other goes first". This is what
  prevents a **deadlock** between the two flags.

The key trick is that each process sets `turn` to the *other* process, i.e. politely yields.
Whoever writes `turn` last loses the race and waits — and exactly one of them writes last.

## Procedure

```java
volatile int     turn;          // whose turn it is
volatile boolean[] taken = new boolean[2];

// process i, other = 1 - i
void lock(int i) {
    int other = 1 - i;
    taken[i] = true;            // I am interested
    turn     = other;           // but you may go first
    while (taken[other] && turn == other)
        ;                       // busy wait
}

void unlock(int i) {
    taken[i] = false;
}
```

Both variables must be **`volatile`**, otherwise the compiler may keep them in registers and the
other process never sees the update.

### Why each requirement holds

| Requirement | Why |
|---|---|
| **Correctness** | `turn` has one value, so at most one of the two loop conditions is false — both cannot be inside. Secured by `taken[]`. |
| **Progress** | An uninterested process has `taken[other] == false`, so the other's loop exits immediately — the flaw of strict alternation is gone. |
| **Fairness** | After leaving, a process that immediately re-requests sets `turn` to the other, so it cannot overtake twice. |
| **No starvation** | Follows from fairness: the wait is bounded by one turn. |

## Complexity / cost

- **Busy waiting** — the waiting process stays Running and burns CPU (see [[spin-lock]]).
- **Two processes only.** Generalising to *n* processes requires the filter/bakery algorithms,
  which the notes do not cover.
- On modern hardware it additionally needs memory barriers; `volatile` alone is not sufficient on
  a weakly-ordered CPU.
  > [!question] Unverified — memory ordering is not discussed in the notes.

## Worked example

```
P0: taken[0]=true; turn=1;          P1: taken[1]=true; turn=0;
```

If P0 writes `turn=1` and then P1 writes `turn=0`, the final value is `turn == 0`. P1's condition
`taken[0] && turn == 0` is true, so **P1 spins**; P0's condition `taken[1] && turn == 1` is false,
so **P0 enters**. Exactly one enters, whichever order the two interleave in.

## Related

- [[mutual-exclusion]] — the property and its four requirements.
- [[lock]] — the interface, and the two failed predecessors.
- [[test-and-set]] — the hardware-supported alternative.
- [[spin-lock]] — the busy waiting it inherits.
- [[race-condition]] — what it eliminates.

## Sources

- [[lecture-bs-3]] (p03, p04)
