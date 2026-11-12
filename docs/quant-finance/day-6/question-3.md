# Day 6 · Hungry Truck Driver

*Edited by Charles and Sophia*

**Brainteaser · Optimization · Staged transport**

[Original question on OpenQuant](https://openquant.co/questions/hungry-truck-driver)

## Problem

A truck driver must move 3000 apples from town $A$ to town $B$, 1000 miles apart.
He eats 1 apple per mile while driving. The truck holds at most 1000 apples,
and he may leave apples anywhere along the road.
What is the maximum number of apples he can deliver to $B$?

The eating rule can be read two ways, and the answers differ:

- **Version 1.** He eats only while apples are **in the truck**, so empty return trips are free.
  This is the literal wording on OpenQuant.
- **Version 2.** He eats on **every** mile driven, loaded or empty. This is the classic
  "camel and bananas" puzzle.

!!! tip "Core idea"
    Move the whole pile forward in stages. The cost per mile depends only on
    **how many loads** the pile needs, so each stage should end exactly when the
    pile drops to the next multiple of 1000.

## Short solution

### Shared idea · Where each stage ends

The number of loads needed is $k=\lceil\text{apples}/1000\rceil$, and the cost per mile
only goes down when $k$ goes down. So:

- While **more than 2000** apples remain, he still needs 3 loads and pays the 3-load rate
  on every mile. Stopping early would only mean paying that higher rate for longer.
- Once the pile reaches **exactly 2000**, it fits in 2 **full** loads and the rate drops.
- Going on to, say, 1500 apples doesn't help: that still takes 2 loads, but neither is full,
  so he pays the 2-load rate to move fewer apples.

The stage boundaries are therefore where **2000** and then **1000** apples remain,
in both versions. Only the cost per mile changes.

### Version 1 · Eats only with apples on board: $k$ per mile

**1. Cost per mile.** Moving a $k$-load pile forward one mile takes $k$ loaded passes
and $k-1$ empty returns. He only eats on the loaded passes:

$$
\text{apples eaten per mile}=k.
$$

**2. Stage 1: 3000 → 2000 apples, $k=3$.**

$$
3x=1000\quad\Longrightarrow\quad x=333\tfrac13\text{ miles}.
$$

**3. Stage 2: 2000 → 1000 apples, $k=2$.**

$$
2y=1000\quad\Longrightarrow\quad y=500\text{ miles}.
$$

**4. Stage 3: one full load to $B$, $k=1$.** He has covered $833\tfrac13$ miles,
so $166\tfrac23$ remain:

$$
1000-166\tfrac23=833\tfrac13.
$$

| Stage | Loads $k$ | Apples per mile | Distance | Apples left |
| --- | --- | --- | --- | --- |
| 1 | 3 | 3 | $333\tfrac13$ | 2000 |
| 2 | 2 | 2 | $500$ | 1000 |
| 3 | 1 | 1 | $166\tfrac23$ | $833\tfrac13$ |

### Version 2 · Eats on every mile: $2k-1$ per mile

**1. Cost per mile.** To move a $k$-load pile forward one mile, he goes forward, back,
forward, back, …, forward. The first $k-1$ loads each need a round trip, and the last
load only goes forward:

$$
\underbrace{2(k-1)}_{k-1\text{ round trips}}+\underbrace{1}_{\text{final forward pass}}=2k-1.
$$

**2. Stage 1: 3000 → 2000 apples, $k=3$, cost $5$ per mile.**

$$
5x=1000\quad\Longrightarrow\quad x=200\text{ miles}.
$$

**3. Stage 2: 2000 → 1000 apples, $k=2$, cost $3$ per mile.**

$$
3y=1000\quad\Longrightarrow\quad y=333\tfrac13\text{ miles}.
$$

**4. Stage 3: one full load to $B$, $k=1$, cost $1$ per mile.** He has covered
$533\tfrac13$ miles, so $466\tfrac23$ remain:

$$
1000-466\tfrac23=533\tfrac13.
$$

| Stage | Loads $k$ | Apples per mile $2k-1$ | Distance | Apples left |
| --- | --- | --- | --- | --- |
| 1 | 3 | 5 | $200$ | 2000 |
| 2 | 2 | 3 | $333\tfrac13$ | 1000 |
| 3 | 1 | 1 | $466\tfrac23$ | $533\tfrac13$ |

!!! success "Answer"
    $$\boxed{\text{Version 1: }833\tfrac13\text{ apples},\qquad \text{Version 2: }533\tfrac13\text{ apples}}$$

    Rounded down to whole apples: $833$ and $533$.

!!! warning "Watch out"
    Before solving, check whether empty return trips cost apples.
    That single detail changes the rate from $k$ to $2k-1$ and the answer from
    $833\tfrac13$ to $533\tfrac13$. The stage boundaries stay at 2000 and 1000 either way.

??? note "Study record"

    | Study record | Value |
    | --- | --- |
    | Question | 3 of 4 |
    | Topic | Brainteaser |
    | Difficulty | Medium |
    | Attempts | 1 |
    | Mastery (Notion notation) | ❚ |
    | Latest attempt | November 12, 2026 |
    | Source | [OpenQuant: Hungry Truck Driver](https://openquant.co/questions/hungry-truck-driver) |

---

[Previous question](question-2.md) · [Day 6 overview](index.md) · [Next question](question-4.md)