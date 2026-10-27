---
title: Page Size
type: concept
tags: [memory, paging, trade-off]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Page Size

## Definition

The size of a page — and therefore of a frame — is a design parameter of [[paging]], always a
power of two `2^m`, because it is exactly the low `m` bits of a virtual address that form the
offset (see [[address-translation]]).

## Context

Belongs to [[paging]]. It is one of the chapter's explicit trade-off questions.

## How it works

| | Small pages | Large pages |
|---|---|---|
| Number of frames in a given memory | **many** | few |
| Size of the [[page-table]] | **large** (many entries) | small |
| Internal [[fragmentation]] | little | **more** (up to one page per process) |
| Number of [[page-fault|page faults]] | more | **fewer** |
| Disk efficiency per fault | poor — many small transfers | **good** — one large transfer |
| Fit to the [[working-set]] | precise | coarse — loads pages the process may not need |

The lecture states the two ends directly:

- **small** → many frames, **large page tables**;
- **large** → **fewer page faults**.

The page-fault-rate argument adds detail (p12): the fault rate depends on the I/O time and the
memory-access characteristics, and both of these **favour larger pages** — because the expensive
part of a disk access is the positioning, not the transfer, so moving more data per fault is
nearly free.

## Related

- [[fragmentation]] — the cost of large pages.
- [[page-table]] / [[multi-level-page-table]] — the cost of small pages.
- [[working-set]] — the size the allocation must cover.
- [[address-width-calculation]] — deriving `m` from the address split.
- [[disk-block]] — the identical trade-off one layer down: large units favour sequential transfer and waste space.

## Common confusions

- **"Fewer page faults" does not mean "less I/O".** Large pages transfer more bytes per fault; the
  win is in the *number* of disk positionings, not the volume.
- **Page size is not chosen per process.** It is fixed by the hardware address split (modern
  machines offer a small set of sizes, but the lecture treats it as one constant).

> [!question] Unverified — the notes contain one line on p13 that reads as "small pages → few page
> faults(?)", which contradicts the rest of the page. The direction given here (small → *more*
> faults) is the one consistent with p12 and with the rest of the notes.

## Sources

- [[lecture-bs-4]] (p12, p13)
- [[lecture-bs-5]] (p16) — the block-size trade-off, structurally the same as this one.
