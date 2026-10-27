---
title: Overview
type: overview
tags: [overview, map]
sources: [lecture-bs-1, lecture-bs-2, lecture-bs-3, lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Operating Systems (Betriebssysteme) — TUHH

A wiki for the Operating Systems module at TUHH, built from handwritten summary notes of the
lecture and the exercises. Pages are in English; German technical terms appear in parentheses on
first use, because the exam is in German.

Start at [[index]] for the full catalogue, or follow the chapter map below.

## Chapter map

| Ch. | Subject | Topic pages | Source |
|---|---|---|---|
| 01 | Foundations, interrupts & I/O, processes | [[operating-system-basics]], [[interrupts-and-io]], [[processes]], [[interprocess-communication]] | [[lecture-bs-1]] |
| 02 | Threads, SMT and signal handling | [[threads]] | [[lecture-bs-2]] |
| 03 | Mutual exclusion, synchronisation and deadlocks | [[concurrency-and-synchronisation]], [[deadlocks]] | [[lecture-bs-3]] |
| 04 | Memory management, paging and virtual memory | [[memory-management]], [[virtual-memory]] | [[lecture-bs-4]] |
| 05 | Scheduling strategies; file systems, allocation and RAID | [[scheduling]], [[file-systems]] | [[lecture-bs-5]] |

All five lecture PDFs are now ingested. Chapters 01–04 map 1:1 onto lectures 1–4; **lecture 5
contains two subjects** and therefore contributes two topic pages under a single chapter number.

## The big picture so far

The module builds upward in layers, each one paying for the convenience the previous one bought.
Lecture 1 lays the first three foundations at once.

**1. What an operating system is.** An [[operating-system]] manages the
[[resource|resources (Betriebsmittel)]] of a machine, controls their allocation, and offers a
[[virtual-machine]]. Its core is the [[kernel]], reached from applications only through the
[[system-call]] door in the [[user-mode-and-kernel-mode]] wall. See
[[operating-system-basics]].

**2. How the hardware talks back.** The [[interrupt]] is the mechanism that lets the CPU stop
polling devices. It carries the whole chain [[interrupt-vector-table]] →
[[interrupt-service-routine]] → [[context-switch]], is ordered by [[interrupt-prioritisation]],
and culminates in [[direct-memory-access]]. Its synchronous sibling, the [[trap]], is what a
[[system-call]] actually is — so layers 1 and 2 meet here. See [[interrupts-and-io]].

**3. What the OS schedules.** A [[process]] is a program in execution, recorded in a
[[process-control-block]] inside a [[process-image]], moving through a [[process-state-model]]
under the direction of the [[scheduler]] and the hand of the [[dispatcher]], via the 8-step
[[process-switch]]. Processes are born from [[fork]], change program with [[exec]], and talk to
each other through [[pipe|pipes]] and [[signal|signals]]. See [[processes]] and
[[interprocess-communication]].

**4. What the OS schedules, cheaply.** Chapter 2 attacks the cost of layer 3: because
[[process-switch|process switching]] must swap memory tables, memory areas and the cache, a
[[thread]] is introduced as a sequential strand of execution *inside* a process, sharing its code
and data segment. A [[thread-switch]] exchanges only the [[thread-control-block]], communication
happens through shared variables instead of [[interprocess-communication|IPC]], and
[[simultaneous-multithreading|SMT]] pushes the same idea into hardware. See [[threads]].

**5. What sharing costs.** Chapter 3 collects the bill for layer 4. Because threads share a data
segment, two strands touching the same datum are in [[conflict]], and if the outcome depends on
the interleaving it is a [[race-condition]]. The remedy is [[mutual-exclusion]] over a
[[critical-section]], built up from [[petersons-algorithm]] and [[test-and-set]] through the
[[spin-lock]] to the blocking [[mutex]], [[condition-variable]], [[monitor]] and [[semaphore]].
Blocking then creates its own failure: a [[deadlock]], a set of processes each waiting for an
event only another member can cause — met with [[deadlock-prevention]],
[[deadlock-detection]] plus [[deadlock-recovery]], or [[deadlock-avoidance]] via the
[[bankers-algorithm]]. See [[concurrency-and-synchronisation]] and [[deadlocks]].

**6. Where it all lives.** Chapters 1–3 allocate one [[resource]] — CPU time. Chapter 4 allocates
the other: **main memory**. [[memory-management]] proceeds as a chain of four variants, each
fixing the previous one's defect: [[static-partitioning]] wastes memory inside its partitions,
[[dynamic-partitioning]] wastes it between them ([[fragmentation]]), [[paging]] drops the
requirement that a process be contiguous and so eliminates the external kind, and
[[virtual-memory]] — paging plus one Present/Absent bit — drops the requirement that a process fit
in memory at all. The price is that every address must be translated on every access, which is why
the [[memory-management-unit]], the [[page-table]] and the [[translation-lookaside-buffer]] exist;
and that a missing page costs a disk access, which is why [[demand-paging]],
[[page-replacement]] ([[lru-page-replacement|LRU]], [[clock-page-replacement|Clock]],
[[not-recently-used|NRU]]), the [[working-set]] and [[thrashing]] fill the rest of the chapter.
The whole edifice rests on [[locality]], first met as a footnote in chapter 1.

**7. Who goes next.** Layers 1–6 assume some process is running, but never say *which*. Chapter 5
closes that hole: [[scheduling]] is the planning of the temporal assignment of processes, decided
by the [[scheduler]] over the [[ready-queue]] and executed by the [[dispatcher]]. The
[[scheduling-goals]] of user and system conflict, so there is no best strategy — only
[[first-come-first-served]] (simple, unfair to short jobs), [[shortest-process-next]] (optimal mean
waiting time, starves long jobs), [[priority-scheduling]] (starves the unimportant, cured by
[[ageing]]) and [[round-robin]] (fair, at the cost of a [[process-switch]] every
[[time-slice]]). The axis that organises all four is
[[preemptive-and-non-preemptive-scheduling]], and the mechanism that makes preemption possible is
the [[clock-interrupt]] — layer 2 returning to serve layer 7.

**8. What survives the power going off.** Chapter 4 manages memory that
[[volatile-and-persistent-memory|vanishes when the power does]]. Chapter 5's second half manages
what does not. A [[file]] is the logical unit, the [[file-management-system]] the manager, and
**transparency** the goal — the same promise the [[virtual-machine]] made in layer 1, now for
storage. The chapter then repeats chapter 4's structure almost exactly: the [[disk-block]] is the
[[page-and-frame|frame]], and the four allocation methods form the same corrective chain —
[[contiguous-allocation]] fragments like [[dynamic-partitioning]], [[linked-list-allocation]]
grows freely but destroys [[locality]], [[file-allocation-table|FAT]] fixes the access time at the
cost of permanent residency, and the [[i-node]] is a [[multi-level-page-table]] per file. Above
this sit the [[directory]], [[mounting]], the [[partition]] and the
[[logical-volume-manager]]; beside it sit the two mechanisms for surviving failure —
[[journaling-file-system|journalling]] for an interrupted write and [[raid]] for a dead disk. The
number that justifies the entire chapter is in [[disk-access-time]]: the same disk delivers
47.62 MiB/s sequentially and 0.958 MiB/s randomly.

## Recurring themes

- **Abstraction vs. capability.** Device-independent interfaces, the [[virtual-machine]], and the
  [[system-call]] boundary all buy convenience and safety at the cost of directness.
- **Don't make the CPU wait.** Programmed I/O → interrupt-driven I/O → [[direct-memory-access]],
  and multiprogramming itself, are all the same optimisation of [[cpu-utilisation]].
- **Don't copy what you won't use.** [[copy-on-write]] for [[fork]]; block transfers justified by
  [[locality]]; [[demand-paging]] loading a page only when it actually faults. Chapter 4 names the
  principle outright by contrasting it with [[prepaging]].
- **Hardware detects, software decides.** The [[memory-management-unit]] notices a missing page;
  the OS chooses the victim. The hardware sets the R and M bits; the OS clears them and reads
  them. The same split runs through [[interrupt]]/[[interrupt-service-routine]] and
  [[test-and-set]]/[[lock]] — put the per-access work in silicon, the per-event policy in code.
- **Cache everything, then measure the hit rate.** [[cache]] for data,
  [[translation-lookaside-buffer]] for translations, [[page-and-frame|frames]] for disk pages —
  three levels of the same idea, all justified by [[locality]] and all evaluated with
  [[hit-rate]].
- **Decision vs. execution.** [[scheduler]]/[[dispatcher]], and the glue routine vs. the handler
  in the [[context-switch]] — the module repeatedly separates *who decides* from *who acts*.
  Chapter 5 confirms it: the strategies are all *policy*, and the eight-step [[process-switch]] is
  all *mechanism*.
- **Write it down before you do it.** [[journaling-file-system|Journalling]] and the write-ahead
  log of the [[raid-consistent-update-problem]] are the same construction at two layers, and
  [[deadlock-recovery]]'s checkpoint-and-roll-back is a third. Whenever an operation must be
  [[atomic-operation|atomic]] but the hardware only offers one small step at a time, the answer is
  a log.
- **Fairness comes from ordering.** The starvation-free mechanisms in the module are exactly those
  with a queue: the strong [[semaphore]], the [[ticket-spin-lock]], [[first-come-first-served]] and
  [[round-robin]]. Everything that chooses on merit — [[shortest-process-next]], static
  [[priority-scheduling]] — starves someone, and the fix ([[ageing]]) works by smuggling waiting
  time back into the merit.
- **Isolation costs, sharing risks.** Private address spaces make processes safe and expensive;
  [[thread|threads]] share everything and are cheap and dangerous. Almost every design choice in
  the module is a point on this line.
- **Waiting: actively or passively.** Chapter 3 turns this into a measurable rule — a
  [[spin-lock]] burns CPU but has no switch cost, a [[mutex]] gives the CPU away but costs two
  [[process-switch|switches]]. The same passive/active axis separates a [[deadlock]] from a
  [[livelock]].
- **Correctness is the easy half.** Every [[mutual-exclusion]] procedure must also satisfy
  progress, fairness and freedom from [[starvation]] — three liveness properties that the naive
  solutions fail while being perfectly correct.

## The distinctions most likely to be examined

1. [[context-switch]] **vs.** [[process-switch]] — a [[system-call]], a mode switch and an
   [[interrupt]] cause only a context switch.
2. [[interrupt]] **vs.** [[trap]] — and the asymmetric may/must/must-never rule for resumption and
   termination.
3. [[process-switch]] **vs.** [[thread-switch]] — PCB + memory table + memory areas + cache flush,
   against a bare TCB exchange.
4. [[mutex]] **vs.** binary [[semaphore]] — **ownership**: only the owner may unlock a mutex, any
   process may release a semaphore.
5. [[deadlock]] **vs.** [[livelock]] **vs.** [[starvation]] — passive (Blocked) vs. active
   (Ready ↔ Running) vs. a single process being permanently passed over.
6. [[deadlock-prevention]] **vs.** [[deadlock-avoidance]] **vs.** [[deadlock-detection]] — exclude
   a condition statically, refuse individual requests at runtime, or clean up afterwards.
7. [[safe-state|Safe]] **vs.** unsafe **vs.** deadlocked — an unsafe state is *not* a deadlock.
8. **Internal vs. external [[fragmentation]]** — inside an allocated area vs. between allocated
   areas. Asked verbatim in exercise 4-1.
9. [[paging]] **vs.** [[virtual-memory]] — virtual memory is paging **plus the Present/Absent
   bit**; under plain paging every page of a running process is resident and no page fault can
   occur.
10. [[cache]] **vs.** [[translation-lookaside-buffer]] — the first caches **data**, the second
    caches **address translations**; and a TLB miss costs one memory access while a
    [[page-fault]] costs a disk access.
11. **Fetch policy vs. replacement policy vs. allocation policy** —
    [[demand-paging]]/[[prepaging]], [[page-replacement]], and
    [[local-and-global-page-replacement]]/[[page-fault-frequency]] answer three different
    questions.
12. [[lru-page-replacement|LRU]] **vs.** [[clock-page-replacement|Clock]] **vs.**
    [[not-recently-used|NRU]] **vs.** [[optimal-page-replacement|optimal]] — on the same table
    they give different victims; exercise 4-6 is built on exactly that.
13. [[swapping]] **vs.** paging out — whole [[process-image|process images]] moved by an OS
    decision, against single pages moved by a [[page-fault]].
14. [[preemptive-and-non-preemptive-scheduling|Preemptive]] **vs.** non-preemptive — *can the CPU
    be withdrawn at any time?* Preemptive costs more overhead but allows a higher number of
    processes; non-preemptive is unusable with interactive users. Asked verbatim in exercise S-1.
15. [[first-come-first-served|FCFS]] **vs.** [[shortest-process-next|SPN]] **vs.**
    [[round-robin|RR]] — on the same arrival table they produce different Gantt charts, different
    mean waiting times and different starvation behaviour. Exercise S-2 is built on exactly that,
    as exercise 4-6 was for page replacement.
16. [[raid-4]] **vs.** [[raid-5]] — one idea apart: RAID 5 distributes the parity so the parity
    disk stops being a write bottleneck.
17. [[raid-0]] **vs.** [[raid-1]] — speed with *reduced* reliability vs. reliability at half the
    capacity. RAID 0 is the only level that makes data less safe than a single disk.
18. [[hot-spare-and-hot-swap|Hot-spare]] **vs.** hot-swap — a reserve *disk* already in the
    machine vs. the *ability* to replace disks during operation.
19. [[file-allocation-table|FAT]] **vs.** [[i-node]] — one table for the whole medium, always
    resident, vs. one index per file, resident only while the file is open.
20. [[journaling-file-system|Journalling]] **vs.** [[raid]] — journalling survives an *interrupted
    write*, RAID survives a *dead disk*. They solve different problems and neither replaces the
    other.
21. **Internal fragmentation on disk** — exercise S-3 c): a 6 KiB file in 4 KiB blocks occupies
    8 KiB. Same concept as item 8, one layer down ([[disk-block]]).
