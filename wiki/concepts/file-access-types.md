---
title: File Access Types
type: concept
tags: [file-system, storage]
chapter: "05"
sources: [lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# File Access Types

## Definition

The two ways a [[file]] can be read or written:

- **Sequential access** (sequentieller Zugriff) — the file is processed from the beginning, block
  after block, in order.
- **Direct access** (direkter Zugriff), also called random access — any position can be addressed
  immediately, without reading what comes before it.

## Context

Belongs to [[file-systems]]. This distinction is the single most important input to every other
decision in the chapter, because the two patterns have wildly different costs on a mechanical disk
and are served well by different allocation methods.

## Why it dominates the chapter

| Design decision | good for sequential | good for direct |
|---|---|---|
| [[disk-block]] size | **large** blocks | smaller blocks (large ones violate [[locality]]) |
| [[contiguous-allocation]] | **excellent** | **excellent** |
| [[linked-list-allocation]] | acceptable | **very poor** — O(n) pointer chase |
| [[file-allocation-table]] | good | **good** (the chain is walked in memory) |
| [[i-node]] | good | good, degrading with file size |
| [[raid-0]] | **excellent** | limited gain |

Exercise S-4 (see [[disk-access-time]]) puts numbers on it: identical hardware delivers
**47.62 MiB/s sequentially and 0.958 MiB/s randomly** — a factor of about 50, entirely because the
mechanical positioning time is amortised over 10 MiB in one case and over 10 KiB in the other.

## Related

- [[disk-access-time]] — the quantitative version of this page.
- [[locality]] — sequential access is spatial locality in its purest form.
- [[file-structures]] — fixed-length records make direct access to record *n* a multiplication.
- [[solid-state-disk]] — removes most of the penalty, because there is nothing to position.
- [[disk-fragmentation]] — turns logically sequential access into physically random access.
- [[memory-mapped-file]] — makes direct access look like a pointer dereference.

## Sources

- [[lecture-bs-5]] (p13, exercise S-4 on p33)
