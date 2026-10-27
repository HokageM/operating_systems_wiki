---
title: Deadlocks
type: topic
tags: [deadlock, livelock, starvation, resources, concurrency]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Deadlocks (Verklemmungen)

## Summary

Blocking solves [[mutual-exclusion|mutual exclusion]] and creates a new failure: a
[[deadlock]] is a set of processes in which **every** process waits for an event that only
another process **in the same set** can cause. Nothing in the set can ever proceed. Its active
twin is the [[livelock]] — the processes keep running but make no progress — and its weaker
relative is [[starvation]], where a single process is permanently passed over.

A deadlock needs four conditions at once: [[mutual-exclusion]], hold-and-wait, no preemption and
circular wait. Remove any one and deadlock becomes impossible. For single-instance
[[consumable-and-reusable-resources|reusable resources]] a deadlock is exactly a cycle in the
[[resource-allocation-graph]]; for multi-instance resources the
[[deadlock-detection-marking-algorithm|marking algorithm]] decides it with matrices.

The OS may respond in four ways: ignore the problem (ostrich), [[deadlock-prevention|prevent]],
[[deadlock-detection|detect]] and [[deadlock-recovery|recover]], or
[[deadlock-avoidance|avoid]] via the [[bankers-algorithm|banker's algorithm]] and the notion of a
[[safe-state]]. The [[dining-philosophers]] problem is the standard illustration of all of it.

## Why it exists

Once a process may **hold** one resource while **waiting** for another, cycles become possible.
The chapter derives this from the joint progress diagram: two processes each needing two
resources in opposite order create a region of the diagram that, once entered, leads
unavoidably to a deadlock — no future scheduling decision can escape it. That is the point of
the whole chapter: deadlock is a *structural* property of the request pattern, not a scheduling
accident.

## Key concepts

- [[deadlock]] — the definition and the four conditions
- [[livelock]] — active waiting instead of passive
- [[starvation]] — permanent unfairness without a cycle
- [[consumable-and-reusable-resources]] — which resource kind can deadlock, and how
- [[resource-allocation-graph]] — the single-instance model
- [[deadlock-prevention]] — exclude one of the four conditions
- [[deadlock-detection]] — find it after the fact
- [[deadlock-recovery]] — abort, successive abort, rollback
- [[deadlock-avoidance]] — refuse dangerous allocations
- [[safe-state]] — the criterion avoidance is built on

## Mechanisms / algorithms

### The four necessary conditions (Coffman)

| Condition | German | Meaning |
|---|---|---|
| **Mutual exclusion** | wechselseitiger Ausschluss | a resource is usable by one process at a time |
| **Hold and wait** | Belegungs- und Wartebedingung | a process holds resources while requesting more |
| **No preemption** | Ununterbrechbarkeit / kein Entzug | resources cannot be taken away, only released voluntarily |
| **Circular wait** | zirkuläres Warten | a closed chain of processes each waiting on the next |

All four must hold **simultaneously**. [[deadlock-prevention]] works by structurally excluding
one of them.

### Deadlock, livelock, starvation

| | [[deadlock]] | [[livelock]] | [[starvation]] |
|---|---|---|---|
| Waiting is | **passive** — Blocked | **active** — Ready ↔ Running | passive or active |
| CPU consumed | none | full | none |
| Cause | circular wait | repeated, symmetric retreat | unfair scheduling or unfair lock |
| Escape without intervention | impossible | possible in principle, never in practice | possible |

The [[process-state-model]] distinction is the crisp exam answer: deadlocked processes sit in
**Blocked**, livelocked processes cycle between **Ready** and **Running**.

### The four strategies

| Strategy | Idea | Cost |
|---|---|---|
| **Ignore** (Vogel-Strauß / ostrich) | do nothing, reboot when it happens | none; used by most general-purpose OSes |
| **[[deadlock-prevention]]** | make one of the four conditions structurally impossible | poor resource utilisation, restrictive programming model |
| **[[deadlock-detection]]** + **[[deadlock-recovery]]** | let it happen, notice it, break it | detection runs repeatedly; recovery loses work |
| **[[deadlock-avoidance]]** | grant a request only if the resulting state stays [[safe-state\|safe]] | needs maximum claims in advance; reduces concurrency |

### The two detection models

- **Single instance per resource:** build the [[resource-allocation-graph]]. A **cycle is
  necessary and sufficient** for deadlock.
- **Multiple instances:** a cycle is only *necessary*. Use the
  [[deadlock-detection-marking-algorithm]] over the resource vector **R**, availability vector
  **V**, request matrix **A** and allocation matrix **Z**. Unmarked processes at the end are
  deadlocked.

The same marking procedure, run hypothetically on the state that *would* result from granting a
request, is the safety test inside the [[bankers-algorithm]].

## Trade-offs

- **Prevention is safe and wasteful.** Preclaiming all resources up front (excluding hold-and-wait)
  works but forces a process to hold resources it will not need for a long time.
- **Linear resource ordering is the cheapest prevention** — it excludes circular wait with no
  runtime cost at all — but it constrains the *programmer*, not the system, so it can only be
  enforced by convention.
- **Detection defers the cost to the failure case**, but choosing *when* to run it is unsolved:
  too often wastes CPU, too rarely lets the deadlock sit.
- **Recovery always destroys work.** Aborting all deadlocked processes is fastest; successive
  abort is gentlest but must re-run detection after each kill; rollback needs checkpoints that
  cost something even when nothing goes wrong.
- **Avoidance needs prophecy.** The [[bankers-algorithm]] requires each process's *maximum*
  future claim, which real programs rarely know.
- **Ignoring is usually right.** Deadlocks are rare in general-purpose systems, and the ostrich
  algorithm costs nothing until they happen — which is why Linux and Windows use it.

## Exam relevance

- *"Was ist ein Deadlock?"* — reproduce both clauses of the [[deadlock]] definition.
- *"Nennen Sie die vier Bedingungen für einen Deadlock."* — the Coffman table. Near-certain.
- *"Unterschied Deadlock, Livelock und Starvation?"* — the passive/active table above.
- *"Welche vier Strategien gibt es?"* — ignore, prevention, detection+recovery, avoidance, each
  with one drawback.
- **Drawing and reading a [[resource-allocation-graph]]**, then naming the processes in the cycle
  — done in exercise 3-4, where the cycle A → P1 → B → P2 → E → P3 → A deadlocks {P1, P2, P3}.
- **Running the [[deadlock-detection-marking-algorithm]]** on given R, Z, A matrices and
  concluding "kein Deadlock" or naming the deadlocked processes. Done twice in exercise 3-5 — this
  is clearly a calculation the exam expects.
- *"Erklären Sie das Philosophenproblem"* — [[dining-philosophers]], including why the naive
  solution deadlocks and the symmetric-retreat fix livelocks.
- *"Wie funktioniert der Bankiersalgorithmus?"* — [[bankers-algorithm]] and [[safe-state]].

## Open questions

- The notes give no rule for **how often** detection should run, only that the question exists.
- **Deadlocks involving consumable resources** (messages that are never sent) are mentioned in the
  resource taxonomy but never modelled; both worked examples use reusable resources.
- No **priority** scheme is discussed for choosing the victim in [[deadlock-recovery]].
- The transcription of the exercise 3-4 operation lists is approximate; the graph, the cycle and
  the conclusion {P1, P2, P3} are certain.
  > [!question] Unverified — the per-process request/release orders in exercise 3-4.

## Sources

- [[lecture-bs-3]] (p13–p21, p26–p28)
