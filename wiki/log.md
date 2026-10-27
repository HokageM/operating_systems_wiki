# Log

Append-only record of ingests, queries, lint passes and schema changes.


## [2026-09-30] ingest | Lecture BS 1 — Exam Overview (Klausuruebersicht)
Source: `raw/lecture/bs_1.pdf`, 23 scanned pages of handwritten German notes (no text layer;
rasterised and read page by page).
Created topics: [[operating-system-basics]], [[interrupts-and-io]], [[processes]],
[[interprocess-communication]]
Created concepts: [[operating-system]], [[resource]], [[virtual-machine]], [[kernel]],
[[locality]], [[device-driver]], [[processor-registers]], [[interrupt]], [[trap]],
[[interrupt-service-routine]], [[interrupt-vector-table]], [[interrupt-prioritisation]],
[[context-switch]], [[direct-memory-access]], [[booting]], [[bios]],
[[user-mode-and-kernel-mode]], [[system-call]], [[process]], [[process-state-model]],
[[swapping]], [[process-control-block]], [[process-image]], [[process-switch]], [[dispatcher]],
[[scheduler]], [[cpu-utilisation]], [[fork]], [[exec]], [[copy-on-write]], [[zombie-process]],
[[orphan-process]], [[pipe]], [[signal]]
Created source page: [[lecture-bs-1]]
Updated: [[overview]] (created, chapter map seeded), [[index]] (40 pages catalogued)
Notes: first ingest, no pre-existing pages to reconcile, no contradictions with the wiki.
One internal tension flagged on [[process]]: p09 counts the attributes as part of the state,
p17 lists code/state/attributes as three constituents.
Open: the exercise sheet "Ex1" referenced on p20-p23 is not in `raw/` - the analysed fork()
program is known only by its behaviour. `raw/exercise/` is still empty.

## [2026-09-30] ingest | Lecture BS 2 - Threads, SMT and Signals
Created: [[threads]], [[thread]], [[thread-control-block]], [[thread-state-model]],
[[thread-switch]], [[simultaneous-multithreading]], [[pthread-api]], [[signal-handler]],
[[lecture-bs-2]]
Updated: [[overview]], [[index]], [[processes]], [[interprocess-communication]], [[process]],
[[process-switch]], [[process-control-block]], [[process-state-model]], [[context-switch]],
[[signal]], [[fork]], [[pipe]], [[processor-registers]], [[interrupt-vector-table]],
[[dispatcher]]
Notes: no contradictions with ch.01 - ch.02 reuses the process-switch cost model as its baseline.
Terminology overlap flagged: "interrupt-vector table" is used both for the CPU-level table
([[interrupt-vector-table]]) and for the per-process signal table ([[signal-handler]]).
Open: exercise sheet 2 itself is not in raw/exercise/; kernel- vs. user-level threads not stated;
no synchronisation primitives yet; no labelled thread state diagram.

## [2026-09-30] ingest | Lecture BS 3 - Mutual Exclusion, Synchronisation and Deadlocks

**Source:** `raw/lecture/bs_3.pdf` (28 pages, scanned handwriting, no text layer) -> [[lecture-bs-3]]

By far the largest chapter so far, and the first with calculations rather than only definitions.
Read via page rasterisation (1500 px PNGs) in batches of four. Splits into synchronisation
(p01-p12), deadlocks (p13-p21) and the exercise recap "BS-KVB-Ex3" (p22-p28).

**New pages (30):**
- Topics (2): [[concurrency-and-synchronisation]], [[deadlocks]]
- Concepts (21): [[conflict]], [[race-condition]], [[mutual-exclusion]], [[critical-section]],
  [[atomic-operation]], [[lock]], [[spin-lock]], [[mutex]], [[condition-variable]], [[monitor]],
  [[semaphore]], [[deadlock]], [[livelock]], [[starvation]],
  [[consumable-and-reusable-resources]], [[resource-allocation-graph]], [[deadlock-prevention]],
  [[deadlock-detection]], [[deadlock-recovery]], [[deadlock-avoidance]], [[safe-state]]
- Algorithms (7, first entries in this category): [[petersons-algorithm]], [[test-and-set]],
  [[ticket-spin-lock]], [[producer-consumer]], [[dining-philosophers]],
  [[deadlock-detection-marking-algorithm]], [[bankers-algorithm]]
- Sources (1): [[lecture-bs-3]]

**Updated (15):** [[overview]] (ch.03 row, fifth big-picture layer, two new recurring themes,
four new likely-examined distinctions), [[index]] (new Algorithms section), [[threads]],
[[processes]], [[interprocess-communication]], [[thread]], [[process]], [[pthread-api]],
[[context-switch]], [[process-switch]], [[process-state-model]], [[resource]], [[scheduler]],
[[pipe]], plus the [[threads]] open question on synchronisation primitives, now resolved.

**Notable:** the chapter answers chapter 2's biggest open question directly. Two worked detection
exercises are reproduced in full on [[deadlock-detection-marking-algorithm]] (state 1: no
deadlock, safe sequence P5 P4 P1 P3 P2; state 2: P3 and P4 deadlocked). Convention flagged for
the exam: in these notes **A is the total claim**, so the outstanding need is **A - Z**.

**Still open:** how often detection should run; victim selection for recovery; priority inversion;
deadlocks over consumable resources; how acquire/release atomicity is achieved in the kernel;
scheduling strategies (presumably ch.04). Exercise sheets themselves are still not in
`raw/exercise/`.

## [2026-09-30] ingest | Lecture BS 4 — Memory Management (Speicherverwaltung)

**Source:** `raw/lecture/bs_4.pdf` — 29 pages of handwritten German notes, scanned without a text
layer, read page by page as rendered images. Chapter heading **BS-KVB**. Four parts: lecture
(p01–p17), exercise recap "BS-KVB-Ex 4" (p18–p24), bonus sheets "Zusatz 1" and "Zusatz 2"
(p25–p28), and a practical sheet "Praxis 4" (p29).

**Created (38 pages):**

- *Topics (2):* [[memory-management]], [[virtual-memory]]
- *Concepts (31):* [[memory-hierarchy]], [[memory-technologies]], [[cache]],
  [[write-through-and-write-back]], [[relocation]], [[physical-and-virtual-memory]],
  [[address-translation]], [[memory-management-unit]], [[static-partitioning]],
  [[dynamic-partitioning]], [[base-and-limit-register]], [[fragmentation]], [[paging]],
  [[page-and-frame]], [[page-table]], [[page-table-entry]], [[multi-level-page-table]],
  [[page-fault]], [[demand-paging]], [[prepaging]], [[dirty-page]],
  [[translation-lookaside-buffer]], [[hit-rate]], [[working-set]], [[thrashing]], [[page-size]],
  [[page-replacement]], [[page-locking]], [[local-and-global-page-replacement]],
  [[page-fault-frequency]], [[address-width-calculation]]
- *Algorithms (4):* [[lru-page-replacement]], [[clock-page-replacement]],
  [[not-recently-used]], [[optimal-page-replacement]]
- *Sources (1):* [[lecture-bs-4]]

**Updated (16 pages):** [[index]], [[overview]] (ch.04 row, a sixth big-picture layer, two new
recurring themes, six new examinable distinctions), [[locality]] (the Lokalitätsprinzip and the
array-traversal exercise), [[swapping]] (whole-process vs. page-level granularity),
[[process-image]], [[process-switch]], [[copy-on-write]], [[direct-memory-access]],
[[resource]], [[interrupt]], [[processes]], [[operating-system-basics]], [[semaphore]] (System V
`semop`), [[pthread-api]] (the thread-safe queue), [[producer-consumer]].

**Notes.** The chapter is a four-step progression — [[static-partitioning]] →
[[dynamic-partitioning]] → [[paging]] → [[virtual-memory]] — in which each variant fixes the
previous one's defect and pays a new price. The exercise sheets are unusually formulaic
(powers of two, bit-field widths, weighted access times), so [[address-width-calculation]] was
created as a dedicated page even though the lecture does not name it as a concept. Exercise 4-6,
which runs LRU / NRU / Clock / optimal over the same four-frame table and gets three different
answers, is the single best revision exercise in the chapter. "Praxis 4" (p29) belongs
thematically to chapter 3 and was folded into [[semaphore]], [[pthread-api]] and
[[producer-consumer]] rather than given chapter-4 pages.

**Conflicts:** none. Chapter 4 extends [[locality]] and [[swapping]] rather than contradicting
them; both pages now state the relationship explicitly.

**Open questions:** placement strategies (first-fit / best-fit / worst-fit / buddy) and
segmentation are never mentioned; FIFO page replacement is not named although exercise 4-6
supplies the "loaded at" column it would need; **scheduling strategies are still absent after four
chapters** — chapter 5 is the last chance. Three transcription uncertainties are flagged in place
with `> [!question]` callouts: the LRU trace on p14, the ≈ 600,025 µs result on p21, and a p13 line
on page size that contradicts p12.

**Wiki size after ingest:** 4 sources, 118 pages, 0 broken links.
