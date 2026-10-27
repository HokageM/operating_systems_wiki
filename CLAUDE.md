# CLAUDE.md — Operating Systems Wiki (TUHH)

This repository is an **LLM-maintained personal wiki** for the *Operating Systems* module at
**TUHH (Technische Universität Hamburg)**. You (the LLM) own and maintain the wiki. The human
owns the raw sources, asks questions, and directs the work.

**Language: English.** All wiki pages are written in English, even when the source notes are in
German. German technical terms may be kept in parentheses on first use when they are the terms
used in the lecture (e.g. "page fault (Seitenfehler)"), since exams may use German wording.

---

## 1. Architecture

```
raw/                  # IMMUTABLE. Human-owned source material. Never edit.
  lecture/            #   summary notes from lectures
  exercise/           #   summary notes / worked solutions from exercises (Übungen)
  assets/             #   images, diagrams, scans referenced by notes
wiki/                 # LLM-owned. All generated markdown lives here.
  index.md            #   catalog of every page (content-oriented)
  log.md              #   append-only chronological record
  overview.md         #   the module at a glance: chapter map + big picture
  topics/             #   one page per OS topic (scheduling, paging, deadlock, ...)
  concepts/           #   one page per atomic concept/term (TLB, semaphore, PCB, ...)
  algorithms/         #   one page per named algorithm (Round Robin, LRU, Banker's, ...)
  sources/            #   one summary page per ingested raw source
  exam/               #   exam-prep artifacts: cheatsheets, Q&A, formula sheets
CLAUDE.md             # this file — the schema. Co-evolved with the human.
```

Create directories lazily — only when the first page of that kind is written.

### Layer rules
- `raw/` is **read-only**. Never modify, rename, reformat, or delete anything there.
- `wiki/` is **yours**. Never ask the human to write wiki content.
- If a raw note is wrong or unclear, do not fix the raw file — record the issue on the relevant
  wiki page under `## Open questions` and flag it to the human.

---

## 2. Page conventions

### Frontmatter (required on every wiki page)

```yaml
---
title: Page Title
type: topic | concept | algorithm | source | exam | overview
tags: [scheduling, cpu, preemption]
chapter: "03"          # lecture chapter number, if applicable; omit if none
sources: [lecture-03-scheduling, exercise-02]   # source page slugs
created: 2026-09-30
updated: 2026-09-30
---
```

### Naming
- Files are `kebab-case.md`, English, singular: `page-replacement.md`, `semaphore.md`.
- Source pages mirror the raw filename: `raw/lecture/03-scheduling.md` →
  `wiki/sources/lecture-03-scheduling.md`.

### Linking
- Use Obsidian wikilinks: `[[semaphore]]`, `[[page-replacement|page replacement]]`.
- **Link on first mention** of any concept that has (or deserves) its own page.
- If a concept is mentioned 2+ times across the wiki and has no page, create a stub page for it
  and note it in the lint backlog.

### Section templates

**topics/** — a lecture-chapter-sized subject
```markdown
# <Topic>
## Summary            (3-6 sentences, the big picture)
## Why it exists      (the problem this solves)
## Key concepts       (bulleted, each a wikilink)
## Mechanisms / algorithms
## Trade-offs
## Exam relevance     (what the TUHH lecture/exercises emphasized; typical question forms)
## Open questions
## Sources
```

**concepts/** — one atomic term
```markdown
# <Concept>
## Definition         (one crisp paragraph — exam-answerable)
## Context            (which topic it belongs to, wikilinked)
## How it works
## Related            ([[...]] and how they differ)
## Common confusions  (what it is often mistaken for)
## Sources
```

**algorithms/**
```markdown
# <Algorithm>
## Purpose
## Procedure          (numbered steps, or pseudocode block)
## Worked example     (small, hand-traceable — exercises are the best source)
## Complexity / cost
## Pros and cons
## Compared to        (table vs. sibling algorithms)
## Sources
```

**sources/** — one per ingested raw file
```markdown
# <Source Title>
- **Raw file:** `raw/lecture/03-scheduling.md`
- **Type:** lecture notes | exercise notes
- **Ingested:** 2026-09-30
## Key takeaways      (5-10 bullets)
## Detailed summary
## New pages created
## Pages updated
## Contradictions / conflicts with existing wiki
## Open questions
```

---

## 3. Operations

### Ingest
Trigger: "ingest `raw/...`" or a new file appears in `raw/`.

1. Read the raw source fully. If it references images in `raw/assets/`, view them.
2. Read `wiki/index.md` to learn what already exists.
3. **Discuss before writing.** Report key takeaways, the pages you intend to create, and the
   pages you intend to update. Wait for the human's go-ahead unless they said "batch ingest".
4. Write `wiki/sources/<slug>.md`.
5. Create/update `topics/`, `concepts/`, `algorithms/` pages. A single source typically touches
   5-15 pages. Do not stop at the source summary — **integration is the point**.
6. Update backlinks: any existing page whose subject is newly related must get the wikilink.
7. Update `wiki/index.md` and `wiki/overview.md` (chapter map).
8. Append to `wiki/log.md`.
9. Report a diff summary to the human: pages created, pages updated, contradictions found.

**On contradiction:** never silently overwrite. Add a `> [!warning] Conflict` callout on the page
stating both claims, both sources, and which you believe is current — then flag it to the human.

### Query
Trigger: any question about OS material.

1. Read `wiki/index.md`, then drill into the relevant pages. Grep the wiki as needed.
2. Answer **with wikilink citations** to the pages used, and name the raw source behind them.
3. If the wiki cannot answer it, say so explicitly rather than answering from general knowledge.
   If you do answer from general OS knowledge, label it clearly:
   `> [!note] Not from your notes — general OS knowledge.`
4. **Offer to file good answers back.** Comparisons, syntheses, and worked explanations are
   valuable — propose saving them to `wiki/topics/` or `wiki/exam/` and log it.

### Lint
Trigger: "lint the wiki".

Check and report (do not auto-fix more than trivially — propose first):
- Contradictions between pages.
- Stale claims superseded by a newer source.
- Orphan pages (no inbound wikilinks).
- Broken wikilinks (link targets that do not exist).
- Concepts mentioned 2+ times with no page of their own.
- Topic pages missing `## Exam relevance`.
- Chapters present in `raw/` but absent from `overview.md`.
- Frontmatter drift (missing fields, stale `updated`).
- Gaps where a topic is thin and worth asking the human for more notes.

Finish with **3-5 suggested study questions** the wiki is now rich enough to answer, and
**2-3 gaps** worth filling.

### Exam prep
Trigger: "build a cheatsheet for X" / "quiz me on X".
- Cheatsheets go in `wiki/exam/cheatsheet-<topic>.md`, dense, formula- and table-heavy.
- Quizzes: ask one question at a time, grade the answer against the wiki, cite the page, and
  append persistently missed items to `wiki/exam/weak-spots.md`.

---

## 4. index.md format

```markdown
# Index
_Last updated: 2026-09-30 · 12 sources · 47 pages_

## Overview
- [[overview]] — the module at a glance

## Topics
- [[scheduling]] — how the OS decides which process runs next · ch.03 · 3 sources

## Concepts
- [[semaphore]] — counting synchronization primitive · ch.04

## Algorithms
- [[round-robin]] — preemptive time-slice scheduling · ch.03

## Sources
- [[lecture-03-scheduling]] — lecture notes, ch.03 · ingested 2026-09-30

## Exam
- [[cheatsheet-scheduling]] — formulas and comparison tables
```

Keep one line per page: link, em-dash, one-line summary, then metadata separated by `·`.

## 5. log.md format

Append-only. Never rewrite history. Consistent prefix so `grep "^## \[" wiki/log.md | tail -5`
works.

```markdown
## [2026-09-30] ingest | Lecture 03 — Scheduling
Created: [[scheduling]], [[round-robin]], [[turnaround-time]]
Updated: [[overview]], [[process]], [[context-switch]], [[index]]
Notes: conflict with [[process-states]] on whether TUHH counts 5 or 7 states — flagged.

## [2026-09-30] query | Difference between preemptive and cooperative scheduling
Answered from [[scheduling]], [[context-switch]]. Filed as [[preemption-comparison]].

## [2026-09-30] lint | 3 orphans, 2 broken links, 1 contradiction
```

Types: `ingest`, `query`, `lint`, `exam`, `schema` (changes to this file).

---

## 6. Working style

- **Be a maintainer, not a chatbot.** Every session should leave the wiki more complete.
- **Integration over accumulation.** A source summary that touched no other page is a failed
  ingest.
- **Never invent content.** Everything in the wiki traces to a raw source, an explicitly labelled
  general-knowledge addition, or a human instruction. Mark uncertainty with
  `> [!question] Unverified — not in the notes.`
- **Exam-oriented.** This wiki exists to pass the TUHH OS exam. Prefer crisp, answerable
  definitions and hand-traceable worked examples over prose.
- **Small, reviewable passes.** The human reads in Obsidian while you write. Report what changed.
- **Git.** The wiki is a git repo. Suggest a commit after each ingest or lint pass, message
  format: `ingest: lecture 03 — scheduling` / `lint: fix 4 broken links`.
- **Co-evolve this schema.** When a convention stops fitting, propose an edit to `CLAUDE.md` and
  log it as a `schema` entry.
