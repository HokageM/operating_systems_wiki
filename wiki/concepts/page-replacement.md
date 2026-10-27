---
title: Page Replacement
type: concept
tags: [memory, virtual-memory, paging]
chapter: "04"
sources: [lecture-bs-4]
created: 2026-09-30
updated: 2026-09-30
---

# Page Replacement

## Definition

**Page replacement** (Seitenersetzung, Verdrängungsstrategie) is the decision **which page to
evict from its frame** when a [[page-fault]] occurs and no frame is free. It is the *replacement
policy*, as opposed to the *fetch policy* ([[demand-paging]] vs. [[prepaging]]) which decides
when to load.

## Context

Belongs to [[virtual-memory]]. The same decision appears one level up as the "Verdrängungs­kriterium"
of a [[cache]], and one level down in the [[translation-lookaside-buffer]], which evicts the entry
unused the longest.

## How it works

Every replacement decision has to answer three questions:

1. **Which page?** — the algorithm: [[lru-page-replacement|LRU]],
   [[clock-page-replacement|Clock]], [[not-recently-used|NRU]], or the unattainable
   [[optimal-page-replacement|optimum]].
2. **Whose pages may be considered?** — see [[local-and-global-page-replacement]].
3. **May this frame be touched at all?** — see [[page-locking]].

If the chosen victim is a [[dirty-page]] it must be written back to persistent storage **before**
the new page can be read in, which doubles the cost of the fault.

### The algorithms of this chapter

| Algorithm | Information used | Cost | Quality |
|---|---|---|---|
| [[optimal-page-replacement]] | the future | not implementable | the benchmark |
| [[lru-page-replacement]] | full access order | high — needs hardware support | very good |
| [[clock-page-replacement]] | one Use bit per frame | low | good approximation of LRU |
| [[not-recently-used]] | R and M bits | very low | acceptable, not optimal |

The progression is a single idea: *approximate LRU ever more cheaply*.

## Related

- [[page-fault]] — the event that triggers replacement.
- [[dirty-page]] — what makes a victim expensive.
- [[page-locking]] — frames excluded from the procedure.
- [[thrashing]] — what no algorithm can fix.

## Common confusions

- **Replacement policy vs. fetch policy.** Exams ask for both by name; LRU is not an answer to
  "which fetch policy?".
- **The best algorithm cannot save an under-provisioned system.** See [[thrashing]].

## Sources

- [[lecture-bs-4]] (p09, p14, p15, p16, p17, p24)
