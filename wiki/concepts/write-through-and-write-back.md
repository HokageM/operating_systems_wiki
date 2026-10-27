---
title: Write-Through and Write-Back
type: concept
tags: [memory, caching]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Write-Through and Write-Back

## Definition

The two policies for keeping a [[cache]] and the level below it consistent on a **write** access.
In both cases the datum is first stored in the cache; they differ in *when* it reaches main memory.

- **Write-through** — every write is passed straight on to main memory as well. Cache and memory
  are always consistent.
- **Write-back** — the write stays in the cache; the line is written to main memory only when it
  is **evicted**. The line must be marked as modified so the hardware knows a write-back is
  needed.

## Context

Belongs to [[cache]]. The same distinction reappears one level down in [[virtual-memory]]: a
[[dirty-page]] must be written back to persistent storage before its frame is reused, an
unmodified page can simply be overwritten. Write-back is the policy in both cases.

## How it works

| | Write-through | Write-back |
|---|---|---|
| Memory traffic | high — one bus transfer per write | low — one transfer per evicted line |
| Consistency | always consistent | temporarily inconsistent |
| Eviction cost | cheap: just drop the line | expensive if the line was modified |
| Bookkeeping | none | a modified/dirty bit per line |

## Related

- [[cache]] — where the policy applies.
- [[dirty-page]] — the same idea at the page level; the M bit of a [[page-table-entry]].
- [[direct-memory-access]] — DMA reads memory directly, so a stale write-back cache is a
  correctness problem; this is one reason for [[page-locking]].

## Common confusions

- **Write-back is not "lazy and therefore worse".** It is the default in practice, because
  [[locality]] means the same line is usually written many times before eviction.
- **Both write into the cache first.** The names describe what happens to *main memory*, not
  whether the cache is updated.

## Sources

- [[lecture-bs-4]] (p02)
