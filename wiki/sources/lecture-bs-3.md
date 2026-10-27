---
title: "Lecture BS 3 — Mutual Exclusion, Synchronisation and Deadlocks"
type: source
tags: [lecture, concurrency, synchronisation, mutual-exclusion, semaphore, deadlock]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Lecture BS 3 — Mutual Exclusion and Deadlocks (BS-MVB)

- **Raw file:** `raw/lecture/bs_3.pdf`
- **Type:** lecture notes (handwritten summary, German) + exercise recap "BS-KVB-Ex3"
- **Ingested:** 2026-09-30
- **Pages:** 28 (scanned handwriting, no text layer — read via page rasterisation)

By far the largest chapter so far, and the first to contain **calculations** rather than only
definitions. It splits cleanly in two: **synchronisation** (p01–p12) and **deadlocks**
(p13–p21), followed by a seven-page exercise recap (p22–p28) that repeats the definitions and
then works two detection problems end to end.

## Key takeaways

- The chapter's framing assumption, stated up front: **no assumption may be made about the
  temporal course of execution**. Everything else follows from it.
- Three verbatim definitions open the chapter and are re-asked in the recap: [[conflict]]
  (two or more processes access the same [[resource]] concurrently), [[race-condition]]
  (zeitkritischer Ablauf — the result depends on the access order), and [[mutual-exclusion]]
  (at most one process in the [[critical-section]] at a time).
- Every mutual-exclusion procedure is judged against **four requirements**: correctness
  (Korrektheit), progress (Fortschrittsbedingung), fairness and no [[starvation]]. This rubric is
  used to kill each naive solution in turn.
- The ladder of solutions: disabling [[interrupt|interrupts]] → lock variables (fail: not
  [[atomic-operation|atomic]]) → strict alternation (fail: violates progress) →
  [[petersons-algorithm]] → [[test-and-set]] → [[spin-lock]] → [[ticket-spin-lock]] →
  [[mutex]] / [[condition-variable]] / [[monitor]] / [[semaphore]].
- The central trade-off: a [[spin-lock]] pays off exactly when the expected wait is **shorter
  than a [[context-switch]]**, because blocking costs **two process switches**.
- [[mutex]] versus binary [[semaphore]] is decided by **ownership**: only a mutex's owner may
  unlock it; any process may release a semaphore, which is what makes cross-process signalling
  possible.
- A [[semaphore]] is a `{ int count; Queue queue; }` with atomic `acquire()`/`release()`
  implemented as [[system-call|system calls]]; `count < 0` means that many processes are blocked;
  **strong** semaphores are FIFO and starvation-free, **weak** ones are not.
- [[condition-variable|Condition variables]] must be tied to a [[mutex]], and the predicate must be
  re-checked in a **`while`** loop — both asked explicitly in the recap.
- A [[deadlock]] is a set of processes in which every process waits for an event that **only
  another process of the same set** can cause. Four conditions are necessary simultaneously:
  mutual exclusion, hold-and-wait, no preemption, circular wait.
- [[deadlock]] waits **passively** (Blocked), [[livelock]] waits **actively** (Ready ↔ Running),
  [[starvation]] hits a **single** process while the system progresses.
- Four strategies: ignore, [[deadlock-prevention]], [[deadlock-detection]] +
  [[deadlock-recovery]], [[deadlock-avoidance]].
- Detection is single-instance (cycle in the [[resource-allocation-graph]], necessary **and**
  sufficient) or multi-instance ([[deadlock-detection-marking-algorithm]] over R, V, A, Z).
- The [[bankers-algorithm]] reuses the marking algorithm as a **hypothetical** safety test, which
  is the whole difference between avoidance and detection.

## Detailed summary

### Part 1 — Mutual exclusion (p01–p06)

Page 1 opens with the goals of concurrency and the three boxed definitions: [[conflict]],
[[race-condition]] and [[mutual-exclusion]]. Page 2 adds [[critical-section]] and
[[atomic-operation]], then the **four requirements** table that structures the rest of the
chapter.

Page 3 introduces the [[lock]] interface (`lock()` / `unlock()`) and two implementations:
disabling [[interrupt|interrupts]] (kernel only, useless on multiprocessors) and
**Sperrvariablen** with **strict alternation** — correct but violating the progress condition.
Page 4 gives [[petersons-algorithm]] in full, with `volatile int turn` and
`volatile boolean[] taken`, and the inline reasoning that `turn` prevents a deadlock between the
two flags while `taken[]` secures correctness.

Page 5 moves to hardware support with [[test-and-set]], defines the [[spin-lock]] and
**active waiting**, notes that [[starvation]] is possible, and states the decision rule: spinning
wins when the wait is shorter than a [[context-switch]]. Page 6 fixes the fairness gap with the
**[[ticket-spin-lock]]** (FIFO), then switches to blocking — which costs **two process switches**
— and defines the [[mutex]] with its owned/not-owned states and the
[[pthread-api|`pthread_mutex_*`]] calls.

### Part 2 — Blocking primitives (p07–p12)

Page 7 defines the [[condition-variable]], its link to a mutex, and
`pthread_cond_init` / `_wait` / `_signal`. Page 8 defines the [[monitor]] (compiler-generated
mutual exclusion) and introduces the [[semaphore]] both as a data structure and as an interface
with atomic `acquire()`/`release()` callable by **different** processes.

Page 9 gives `struct Semaphore { int count; Queue queue; }`, distinguishes binary from counting
semaphores, and tabulates the meaning of `count` being positive, zero or negative. Page 10
contrasts **strong** (FIFO) with **weak** semaphores, traces a worked acquire/release sequence,
and notes that semaphores are implemented as [[system-call|system calls]]. Page 11 is the
**mutex versus binary semaphore** comparison — ownership, who may release, and the fact that a
semaphore is "a pure counter".

Page 12 works the [[producer-consumer]] bounded queue in Java with a `ReentrantLock` and the two
Conditions `full` and `empty`, stressing the **`while`** rather than `if` around `await()`.

### Part 3 — Deadlocks (p13–p21)

Page 13 defines the [[deadlock]] with both clauses, splits resources into
[[consumable-and-reusable-resources|consumable and reusable]], and draws the joint progress
diagram with its unavoidable deadlock region. Page 14 contrasts deadlock (passive, Blocked) with
[[livelock]] (active, Ready ↔ Running) and [[starvation]], and lists the **four conditions**.

Page 15 introduces the [[resource-allocation-graph]] — nodes, instances, allocation and request
edges, and the cycle criterion for single-instance resources — then poses the
[[dining-philosophers]] problem with both its deadlock and livelock variants. Page 16 gives the C
solution with `state[]`, a binary `lock` semaphore and per-philosopher semaphores `s[N]`
(`take_forks` / `put_forks` / `check_forks`), noting that it removes livelock but **not**
starvation.

Page 17 lists the four strategies and works through [[deadlock-prevention]] by excluding each of
the four conditions — preclaiming, forced resource release, linear resource ordering — with the
drawback of each. Page 18 covers [[deadlock-detection]]: the single-instance graph criterion and
when to check, then the multi-instance model with **R**, **V**, **A**, **Z** and the relations
between them. Page 19 gives the four-step
[[deadlock-detection-marking-algorithm]] plus a full worked example with `R = (4 2 3 1)`,
`V = (2 1 0 0)` ending in "kein Deadlock". Page 20 covers [[deadlock-recovery]]: process abort,
successive abort, checkpointing/rollback. Page 21 covers [[deadlock-avoidance]] — denial of
process initiation and the [[bankers-algorithm]] with [[safe-state|safe and unsafe states]],
using the marking algorithm as the safety check.

### Part 4 — Exercise recap "BS-KVB-Ex3" (p22–p28)

**3-1 (p22–p23)** re-asks the definitions: [[conflict]], [[race-condition]],
[[mutual-exclusion]] with its four requirements, then [[spin-lock]] pros and cons,
[[semaphore]], [[mutex]] and [[deadlock]].

**3-2 (p24)** is a mutual-exclusion exercise with `turn` and `blocked[2]`, in which an
interleaving is constructed that puts **both** processes into the critical section — i.e. the
proposed procedure violates **correctness**. The transcription of the trace is approximate; the
conclusion is certain.

**3-3 (p25)** is the [[condition-variable]] exercise, quoting the Java contract verbatim and
answering the two classic questions: **why condition variables are tied to a lock** (the thread
blocks inside the critical section, so the lock must be released for the duration of the blocking
and re-acquired afterwards, or nobody could ever make the condition true) and **why `while` and
not `if`** (the woken thread does not yet hold the mutex, so another thread may change the state
before it runs).

**3-4 (p26)** builds a [[resource-allocation-graph]] from the operation traces of P1…P4 over
resources A…F and finds the cycle `A → P1 → B → P2 → E → P3 → A`, deadlocking **{P1, P2, P3}**;
P4 is not involved.

**3-5 (p27–p28)** runs the [[deadlock-detection-marking-algorithm]] on two states:

- **State 1:** `R = (10 9 8 7)`, five processes. `V = (3 3 2 1)`, marking order P5, P4, P1, P3,
  P2 → all marked → **kein Deadlock**, the state is [[safe-state|safe]].
- **State 2:** `R = (7 5 9 6)`, four processes. `V = (3 2 0 3)`, only P2 and P1 can be marked →
  **P3 and P4 are deadlocked**, "Deadlock unvermeidbar".

Both are reproduced in full on [[deadlock-detection-marking-algorithm]].

## New pages created

Topics: [[concurrency-and-synchronisation]], [[deadlocks]]

Concepts: [[conflict]], [[race-condition]], [[mutual-exclusion]], [[critical-section]],
[[atomic-operation]], [[lock]], [[spin-lock]], [[mutex]], [[condition-variable]], [[monitor]],
[[semaphore]], [[deadlock]], [[livelock]], [[starvation]],
[[consumable-and-reusable-resources]], [[resource-allocation-graph]], [[deadlock-prevention]],
[[deadlock-detection]], [[deadlock-recovery]], [[deadlock-avoidance]], [[safe-state]]

Algorithms (the first pages in this category): [[petersons-algorithm]], [[test-and-set]],
[[ticket-spin-lock]], [[producer-consumer]], [[dining-philosophers]],
[[deadlock-detection-marking-algorithm]], [[bankers-algorithm]]

## Pages updated

[[overview]] (ch.03 row and a fifth big-picture layer), [[index]] (new Algorithms section),
[[threads]], [[processes]], [[interprocess-communication]], [[thread]], [[process]],
[[pthread-api]], [[context-switch]], [[process-state-model]], [[resource]], [[signal]],
[[process-switch]], [[scheduler]], [[pipe]]

## Contradictions / conflicts with existing wiki

None. Chapter 3 is the direct sequel to chapter 2 and answers its most prominent open question.

Two things worth recording as *resolutions* rather than conflicts:

- [[threads]] closed with *"synchronisation primitives are the obvious sequel to
  shared-address-space communication but are not in this source — presumably chapter 3."* This is
  now confirmed: chapter 3 is exactly that.
- The cost model established in [[process-switch]] and reused in [[thread-switch]] reappears here
  as the **quantitative basis** for the spin-versus-block decision. It is used consistently, not
  contradicted.

One terminology note: **A** in this chapter's matrices is the **total claim**, not the outstanding
request — the outstanding need is `A − Z`. Textbooks that call the outstanding request "Request"
and the claim "Max" use the opposite convention, so exam answers should follow the notes.

## Open questions

- The exercise sheet itself ("Ex3") is not in `raw/exercise/` — only this recap, so the original
  problem statements for 3-2 and 3-4 are not traceable.
- **How often** deadlock detection should run is raised on p18 and never answered.
- **Victim selection** for [[deadlock-recovery]] is not given a rule.
- **Priority inversion**, the classic [[mutex]] failure mode, is never mentioned.
- Deadlocks over **consumable** resources are named in the taxonomy but never modelled.
- The notes do not say how the atomicity of `acquire()`/`release()` is achieved inside the
  [[kernel]].
  > [!question] Unverified — presumably [[test-and-set]] or disabled [[interrupt|interrupts]].
- **Barriers**, **reader/writer locks** and **lock-free** techniques are absent.
- The **scheduling strategies** hinted at by the [[scheduler]] page are still not covered —
  presumably chapter 4.
- The p24 interleaving trace and the p26 operation lists are approximate transcriptions; their
  conclusions (correctness violated; deadlock over {P1, P2, P3}) are certain.
  > [!question] Unverified — exact operation orders in exercises 3-2 and 3-4.

## Sources

- Raw: `raw/lecture/bs_3.pdf`, pages 1–28.
