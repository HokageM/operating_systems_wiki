---
title: Locality
type: concept
tags: [basics, memory, caching]
chapter: "01"
sources: [lecture-bs-1, lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Locality

## Definition

The **locality property** (Lokalitätseigenschaft) states that the probability that data currently
in use will be needed again in the near future is **very high**.

Two forms are distinguished:

- **Temporal locality** (zeitliche Lokalität) — data used now is likely to be used again *soon*.
- **Spatial locality** (räumliche Lokalität) — data *near* the data used now is likely to be used
  soon.

## Context

Belongs to [[operating-system-basics]]. It is an empirical observation about program behaviour,
not a rule programs must obey — but it is the justification for essentially every cache and
prefetch decision an OS makes.

## How it works

Because locality holds, it pays to keep recently used data in a small fast store and to fetch data
in *blocks* rather than single words:

- CPU caches and the TLB exploit temporal locality.
- Loading a whole block via [[direct-memory-access]] rather than one word at a time exploits
  spatial locality.
- Page replacement strategies and [[swapping]] decisions rest on the assumption that the recent
  past predicts the near future.

## The locality principle in chapter 4

Chapter 4 gives the property its exam name — the **Lokalitätsprinzip** — and a sharper wording
(4-1 e):

> Memory accesses for instructions and data **concentrate on narrow memory areas** during
> execution.

Everything in [[memory-management]] and [[virtual-memory]] rests on it: the
[[memory-hierarchy]] itself, the [[cache]], the [[translation-lookaside-buffer]], the
[[hit-rate]] it produces, the [[working-set]] as its operational form, and
[[lru-page-replacement|LRU]] as the assumption "recent past ⇒ near future" turned into an
algorithm. A program that violates it [[thrashing|thrashes]].

### The traversal exercise (4-2, p12/p20)

```c
auto a = new int[100][100];

// P1: row-wise — matches the storage order
for (int i = 0; i < 100; i++)
    for (int j = 0; j < 100; j++)
        a[i][j] = i * j;        // good locality, few page faults

// P2: column-wise — jumps one row width per step
for (int j = 0; j < 100; j++)
    for (int i = 0; i < 100; i++)
        a[i][j] = i * j;        // "Lokalitätsprinzip verletzt" -> more page faults, slower
```

Arrays are stored **row-wise**, so P1 walks through contiguous addresses while P2 strides across
the whole array on every step. With a large enough array (the lecture uses `int[1000][1000]`),
each step of P2 touches a different [[page-and-frame|page]] — so the same computation produces
vastly more [[page-fault|page faults]]. This is the canonical exam answer: **name the storage
order, then name the consequence.**

## Related

- [[direct-memory-access]] — block-wise transfer is spatial locality made profitable.
- [[swapping]] — deciding what to evict assumes locality.
- [[processor-registers]] — the smallest, fastest level of the same hierarchy.
- [[memory-hierarchy]] — the whole structure locality justifies.
- [[working-set]] — locality made concrete and measurable.
- [[hit-rate]] — locality made quantitative.
- [[thrashing]] — what happens when it fails.
- [[disk-block]] — large blocks exploit locality sequentially and violate it under random access.
- [[disk-access-time]] — the price of violating locality on disk: a factor of about 50.
- [[disk-fragmentation]] — what destroys locality on a file system.
- [[linked-list-allocation]] — explicitly criticised in the notes for violating the locality principle.

## Common confusions

- **Locality is probabilistic, not guaranteed.** A program with random access patterns defeats it.
- **Temporal vs. spatial** — exams usually ask for *both* names plus a one-line explanation each;
  naming only one costs points.
- **Locality is a property of the *program*, not of the hardware.** The hardware only exploits it;
  a badly written loop destroys it, as exercise 4-2 shows.

## Sources

- [[lecture-bs-1]] (p02)
- [[lecture-bs-4]] (p12, p19, p20)
- [[lecture-bs-5]] (p16–p17) — locality as the criterion for block size and allocation method.
