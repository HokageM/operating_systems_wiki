---
title: Ticket Spin Lock
type: algorithm
tags: [concurrency, mutual-exclusion, algorithm, fairness, spin-lock]
chapter: "03"
sources: [lecture-bs-3]
created: 2026-09-30
updated: 2026-09-30
---

# Ticket Spin Lock

## Purpose

Make a [[spin-lock]] **fair**: serve waiting processes in **FIFO order** so that no process can be
overtaken indefinitely, eliminating the [[starvation]] that plain [[test-and-set]] spinning
allows.

## Idea

The bakery/deli metaphor: on arrival you **draw a ticket**; a display shows which number is
**now being served**. You wait until your number comes up, and on leaving you advance the
display. Because tickets are handed out in arrival order, service is strictly FIFO.

## Procedure

```c
typedef struct {
    int next_ticket;   /* next number to hand out */
    int now_serving;   /* number currently admitted */
} ticket_lock_t;

void lock(ticket_lock_t *l) {
    int my = fetch_and_add(&l->next_ticket, 1);   /* atomic */
    while (l->now_serving != my)
        ;                                          /* spin */
}

void unlock(ticket_lock_t *l) {
    l->now_serving++;
}
```

Drawing the ticket must be [[atomic-operation|atomic]] — a fetch-and-add, the counting relative of
[[test-and-set]] — otherwise two processes could draw the same number.

## Complexity / cost

| | Plain [[spin-lock]] | **Ticket spin lock** |
|---|---|---|
| Correctness | yes | yes |
| Fairness | **no** — arbitrary winner | **yes** — strict FIFO |
| [[starvation]] | possible | **impossible** |
| Waiting | busy | busy |
| Extra state | one word | two words |

It satisfies all four requirements of [[mutual-exclusion]], including the fourth, which the plain
spin lock does not. The cost is still **busy waiting** — it fixes the fairness problem, not the
CPU-waste problem. For the latter one must block, i.e. use a [[mutex]] or [[semaphore]].

It is the same fairness idea as a **strong** (FIFO) [[semaphore]], expressed at the spinning
level.

## Related

- [[spin-lock]] — the unfair base version.
- [[test-and-set]] — the atomic primitive family.
- [[starvation]] — what it eliminates.
- [[mutual-exclusion]] — requirements 3 and 4.
- [[semaphore]] — strong semaphores use the same FIFO guarantee.

## Sources

- [[lecture-bs-3]] (p06)
