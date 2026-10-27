---
title: Demand Paging
type: concept
tags: [memory, paging, virtual-memory]
chapter: "04"
sources: [lecture-bs-4, lecture-bs-5]
created: 2026-09-30
updated: 2026-09-30
---

# Demand Paging

## Definition

**Demand paging** is the *fetch policy* (Ladestrategie) under which a page is loaded into main
memory **only when it is actually needed** — that is, only in response to a [[page-fault]].
Nothing is loaded speculatively.

## Context

Belongs to [[virtual-memory]]. It is one of the two fetch policies the lecture names, the other
being [[prepaging]]. Fetch policy answers *when* to load; [[page-replacement]] answers *what to
throw out*.

## How it works

A process starts with **no pages resident**. Its very first instruction faults, that page is
loaded, execution continues, the next unmapped reference faults, and so on. After a short warm-up
the process's [[working-set]] is resident and the fault rate drops sharply — precisely because of
[[locality]].

| | |
|---|---|
| (+) | never loads a page that is not used — no wasted I/O, no wasted frames |
| (+) | a process can start before its whole image has been read from disk |
| (−) | a burst of page faults at process start (and after every [[process-switch]]) |
| (−) | each fault is a separate, small disk access — expensive per byte |

## Related

- [[prepaging]] — the opposite policy, loading pages before they are demanded.
- [[page-fault]] — the trigger.
- [[working-set]] — what demand paging converges on.
- [[copy-on-write]] — the same "don't do work until it is provably needed" principle applied to
  [[fork]].
- [[memory-mapped-file]] — demand paging applied to a file: mapping it loads nothing, and blocks arrive one fault at a time.

## Common confusions

- **Demand paging is a *fetch* policy, not a replacement policy.** [[lru-page-replacement|LRU]],
  [[clock-page-replacement|Clock]] and [[not-recently-used|NRU]] answer a different question and
  are used *together* with demand paging, not instead of it.
- **"On demand" means on page fault**, not "when the programmer asks".

## Sources

- [[lecture-bs-4]] (p09, p14)
- [[lecture-bs-5]] (p13) — memory-mapped files are demand-paged.
