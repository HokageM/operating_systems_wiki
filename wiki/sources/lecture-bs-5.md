---
title: "Lecture BS 5 — Scheduling and File Systems"
type: source
tags: [lecture, scheduling, file-system, raid]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Lecture BS 5 — Scheduling and File Systems

**Raw source:** `raw/lecture/bs_5.pdf` (35 pages, handwritten German summary notes, no text layer —
read by rasterising each page and viewing it).

**Header on the sheets:** `BS-KVB` for the lecture part, `BS-KVB-ÜS` for the exercise recap.

## Key takeaways

1. **Lecture 5 is two chapters in one.** Pages 01–09 finish CPU [[scheduling]]; pages 10–27 are an
   entirely new subject, [[file-systems]]. The exercise recap (p28–p35) covers both. This is the
   first lecture in the module that does not map 1:1 onto a chapter — see the note on chapter
   numbering below.
2. **The scheduling half resolves a question standing since chapter 01.** [[scheduler]] and
   [[dispatcher]] were defined there with the explicit open question "the strategies are not yet
   covered". They are covered here: [[first-come-first-served]], [[shortest-process-next]],
   [[priority-scheduling]] and [[round-robin]], organised by the
   [[preemptive-and-non-preemptive-scheduling|preemptive/non-preemptive]] distinction.
3. **Every scheduling algorithm is a different answer to the same conflict.** The
   [[scheduling-goals]] of the user (response time) and of the system (throughput, utilisation,
   fairness, low overhead) cannot all be maximised at once. SPN buys mean waiting time with
   [[starvation]]; Round-Robin buys fairness with overhead; FCFS buys simplicity with everything
   else.
4. **The file-system half rhymes with chapter 04 throughout.** [[disk-block]] is [[page-and-frame]];
   the [[disk-block|block-size]] trade-off is the [[page-size]] trade-off; [[i-node]] is
   [[multi-level-page-table]]; [[sparse-file]] is an unmapped page range; [[contiguous-allocation]]
   fails for exactly the reason [[dynamic-partitioning]] failed. Naming these parallels is the
   fastest way to learn the chapter.
5. **The four allocation methods form a progression**, each fixing the previous one's defect:
   contiguous (fast, cannot grow) → linked list (grows, slow random access) → FAT (fast, but the
   whole index is resident) → i-node (per-file index, resident only when open).
6. **RAID is a two-axis design space**: speed ([[raid-0]]) versus reliability ([[raid-1]]), with
   [[parity-and-xor|parity]] levels ([[raid-4]], [[raid-5]], [[raid-6]]) as the capacity-efficient
   compromise. RAID 4 → 5 is one idea only: *stop putting the parity on one disk*.
7. **Redundancy needs its own consistency mechanism.** [[raid-consistent-update-problem]] and
   [[journaling-file-system]] are the same technique — write ahead, then write for real — at two
   different layers. That parallel is the single best synthesis point in the chapter.
8. **The exercises are calculation-heavy and highly exam-representative:** [[i-node]] sizing (S-3),
   [[disk-access-time]] (S-4), [[raid-0]]/[[raid-5]] address mapping (S-5), and
   [[raid-parity-update]] (S-6).

## Detailed summary by part

### Part A — Scheduling (p01–p09)

p01 opens by separating the two roles from chapter 01: the **[[dispatcher]]** realises the
switching, the **[[scheduler]]** decides *to which* process is switched. Process scheduling is "the
planning of the temporal assignment of processes". The [[scheduling-goals]] are split into the
user's and the system's, and the
[[preemptive-and-non-preemptive-scheduling|preemptive/non-preemptive]] distinction is introduced
with its trade-off — *preemptive = greater overhead but a higher number of processes*. Voluntary
yielding is named [[cooperative-scheduling]].

p02–p03 give the two implementation bases — **priority-based** ([[priority-scheduling]]) and
**time-slice based** via the [[clock-interrupt]], the latter always preemptive — and the three
classification axes for priorities: external/internal, static/dynamic,
preemptive/non-preemptive. The **mixed form** withdraws the CPU on expiry of a
[[time-slice]] *or* when a higher-priority process becomes ready.

p04–p05 treat the non-preemptive procedures, which apply to **batch jobs with known service
times**. [[first-come-first-served]] manages the [[ready-queue]] as a FIFO list and is criticised
for disadvantaging short and I/O-heavy processes; the worked example (A compute-bound, B and C
I/O-bound) shows that the *order* dominates the average waiting time.
[[shortest-process-next]] guarantees the shortest mean waiting time but requires the processing
time to be known and can starve long processes. Non-preemptive [[priority-scheduling]] starves
low-priority processes, with **[[ageing]]** given as the remedy.

p06–p09 treat the preemptive procedures. [[round-robin]] gives each of *n* processes **1/n of the
CPU in units of size q**, with **no process waiting longer than (n−1)·q** — fair, simple and
widely used; the critical point is the choice of q, and the quiz answer on p07 states that *CPU
utilisation by application code rises with growing q*. Preemptive priority scheduling inserts
arrivals by priority and re-inserts the interrupted process into the queue. p08 spells out the
Round-Robin queue discipline that exercise S-2 tests: **both preempted processes and new arrivals
go to the end of the queue.**

### Part B — File systems (p10–p27)

p10 starts from the medium: [[volatile-and-persistent-memory]], then the
[[solid-state-disk]] with its consequences of having no mechanical parts, its whole-block
read/write restriction and [[wear-levelling]]; SATA for HDD, NVMe for SSD.

p11–p13 build the abstraction. A **[[file]]** is a *logical unit for storing information on
external media, with a name and administration information*; the
[[file-management-system]] provides the services and unifies persistent storage towards
applications, with **transparency** as its stated goal and five named tasks.
[[file-structures]] distinguishes unstructured from structured, noting that structured files were
displaced by relational databases. [[file-system-architecture]] gives the five layers and the five
criteria for file organisation. p13 covers [[file-access-types]],
[[asynchronous-file-access]] and the [[memory-mapped-file]] with its four advantages and one
disadvantage.

p14–p16 organise storage. [[file-metadata]], [[directory]], and the two alternatives for several
devices — drive letters versus [[mounting]]. Then the two ways of carving storage:
[[logical-volume-manager]] (dynamic, combining, resizable at runtime) and [[partition]] (static,
dividing, one file system each). p16 introduces the [[disk-block]], its size trade-off against
[[locality]], [[disk-fragmentation]], and the three tasks of persistent-storage management.

p17–p19 are the allocation methods: [[contiguous-allocation]], [[linked-list-allocation]],
[[file-allocation-table]] and [[i-node]] (with the classic
`(12 + 256 + 256² + 256³) × block size`), plus [[sparse-file]] and
[[journaling-file-system]].

p20–p27 are [[raid]]. The definition and the decomposition procedure, hardware versus software
controllers, then the levels: [[raid-0]], [[raid-1]], [[nested-raid]],
[[raid-consistent-update-problem]] with its write-ahead log, [[raid-4]] (parity disk bottleneck),
[[raid-5]] (parity distributed), [[raid-6]] (two independent parities, (n−2)/n), and finally
[[hot-spare-and-hot-swap]].

### Part C — Exercise recap (p28–p35)

- **S-1** — the [[scheduling-goals|decisive requirement]] of an interactive system; the
  [[preemptive-and-non-preemptive-scheduling|preemptive/non-preemptive]] difference; and the
  **eight steps of a [[process-switch]]**, with a thread switch being *the same without step 5*
  (no MMU preparation, since [[threads]] share the address space).
- **S-2** — a full Gantt-chart exercise (A 2/4, B 0/3, C 4/2, D 1/6) worked under
  [[first-come-first-served]], [[shortest-process-next]] and [[round-robin]] with q = 2, including
  the evolving [[ready-queue]].
- **S-3** — [[i-node]] sizing: maximum file size, administration blocks, and how many 6 KiB files
  fit in 2 GiB with 4 KiB blocks ([[disk-block]], internal [[fragmentation]]).
- **S-4** — [[disk-access-time]]: 47.62 MiB/s sequential versus 0.958 MiB/s random on identical
  hardware, and 100 IOPS.
- **S-5** — address mapping for [[raid-0]] (`A mod n`, `A / n`) and for [[raid-5]] (divide by
  **n−1**, shift past the rotating parity block).
- **S-6** — the additive and subtractive methods of [[raid-parity-update]], with the break-even at
  n = 4.

## New pages created

**Topics (2):** [[scheduling]], [[file-systems]]

**Algorithms (6):** [[first-come-first-served]], [[shortest-process-next]], [[round-robin]],
[[priority-scheduling]], [[i-node]], [[raid-parity-update]]

**Concepts — scheduling (7):** [[scheduling-goals]],
[[preemptive-and-non-preemptive-scheduling]], [[cooperative-scheduling]], [[time-slice]],
[[clock-interrupt]], [[ready-queue]], [[ageing]]

**Concepts — file systems (23):** [[volatile-and-persistent-memory]], [[solid-state-disk]],
[[wear-levelling]], [[file]], [[file-management-system]], [[file-structures]],
[[file-system-architecture]], [[file-access-types]], [[asynchronous-file-access]],
[[memory-mapped-file]], [[file-metadata]], [[directory]], [[mounting]],
[[logical-volume-manager]], [[partition]], [[disk-block]], [[disk-fragmentation]],
[[disk-access-time]], [[contiguous-allocation]], [[linked-list-allocation]],
[[file-allocation-table]], [[sparse-file]], [[journaling-file-system]]

**Concepts — RAID (9):** [[raid]], [[raid-0]], [[raid-1]], [[nested-raid]], [[raid-4]],
[[raid-5]], [[raid-6]], [[parity-and-xor]], [[raid-consistent-update-problem]],
[[hot-spare-and-hot-swap]]

## Pages updated

- [[scheduler]], [[dispatcher]] — the chapter-01 open question "strategies not yet covered" is
  **resolved**; both now link into [[scheduling]].
- [[processes]] — scheduling section completed and its open question closed.
- [[process-switch]] — reconciled with the **eight-step list** from exercise S-1 c), and the
  thread-switch delta (skip the MMU step) added.
- [[context-switch]], [[process-state-model]], [[process-control-block]] — preemption as the
  Running → Ready transition; the clock interrupt as its trigger.
- [[threads]] — the thread switch omits step 5; cheaper precisely because the address space is
  shared.
- [[starvation]] — new instances from SPN and static priorities; [[ageing]] as the cure.
- [[interrupt]], [[interrupts-and-io]] — the [[clock-interrupt]] as the OS's own interrupt source.
- [[locality]], [[fragmentation]], [[page-size]], [[memory-hierarchy]] — the file-system
  counterparts.
- [[virtual-memory]], [[page-fault]], [[demand-paging]], [[page-locking]] — the
  [[memory-mapped-file]] connection.
- [[page-table]], [[multi-level-page-table]] — the [[file-allocation-table]]/[[i-node]] parallel.
- [[direct-memory-access]] — [[asynchronous-file-access]].
- [[atomic-operation]], [[deadlock-recovery]] — the write-ahead-log pattern.
- [[operating-system]], [[resource]] — storage as the fourth managed resource.
- [[index]], [[overview]], [[log]].

## Contradictions and corrections

> [!warning] Conflict — administration block count in exercise S-3 b)
> The notes record **2056 KiB** (514 blocks) for the administration structures of a maximum-size
> file. Counting the i-node block itself gives **515 blocks = 2060 KiB**; counting only the *index*
> blocks (1 single-indirect + 1 first-level double-indirect + 512 second-level) gives 514. The
> discrepancy is whether the i-node is counted. Both readings are defensible — state the breakdown
> explicitly in the exam rather than just the total. See [[i-node]].

> [!warning] Conflict — chapter numbering
> Chapters 01–04 mapped 1:1 onto lectures 1–4. Lecture 5 contains **two** subjects, so this wiki
> files both [[scheduling]] and [[file-systems]] under `chapter: "05"` rather than splitting them
> into 05 and 06. If the official slide deck numbers file systems as its own chapter, the
> frontmatter of the file-system pages will need adjusting — but no content would change.

No factual contradiction with chapters 01–04 was found. The eight-step [[process-switch]] list
from S-1 c) **refines** rather than contradicts the earlier description, and the thread-switch
delta confirms what [[threads]] already claimed about the cost of a thread switch.

## Open questions

> [!question] Unverified — not in the notes
> **Disk-arm scheduling** (FCFS, SSTF, SCAN/elevator, C-SCAN) is absent, even though
> [[file-system-architecture]] names "scheduling of accesses" as a duty of the I/O supervisor and
> S-4 quantifies exactly the seek times such algorithms minimise. Conspicuous, given that CPU
> scheduling is covered in the same lecture. Check the official slides.

> [!question] Unverified — not in the notes
> **Multi-level feedback queues**, **guaranteed/lottery/fair-share scheduling** and
> **real-time scheduling** (EDF, rate-monotonic) are not covered; nor is **multiprocessor
> scheduling**, despite [[simultaneous-multithreading]] appearing in chapter 02.

> [!question] Unverified — not in the notes
> **Free-space management** (bitmaps versus free lists), **directory implementation**, **hard and
> symbolic links**, and any real treatment of **access rights** beyond the mention in
> [[file-metadata]].

> [!question] Still open from chapter 04
> Placement strategies (first-fit / best-fit / worst-fit / buddy) and **segmentation** were never
> mentioned, and FIFO page replacement was left unnamed. Lecture 5 does not resolve these.

## Sources

- `raw/lecture/bs_5.pdf`
