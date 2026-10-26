# Day 1 · Race to Ace

| Study record | Value |
| --- | --- |
| Question | 2 of 4 |
| Topic | Probability |
| Difficulty | Medium |
| Attempts | 1 |
| Mastery (Notion notation) | ❚ |
| Latest attempt | October 22, 2026 |
| Source | [OpenQuant: Race to Ace](https://openquant.co/questions/race-to-ace) |

## Expected Number of Draws Until the First Ace

### Problem

What is the expected number of cards drawn from a standard 52-card deck until the first ace appears (including the ace)?

### Solution: Symmetry + Linearity of Expectation

A deck contains **4 aces** and **48 non-ace cards**.

The 4 aces divide the 48 non-ace cards into **5 gaps**:

`Gap 1 | A₁ | Gap 2 | A₂ | Gap 3 | A₃ | Gap 4 | A₄ | Gap 5`

Let Gᵢ denote the number of non-ace cards in the i-th gap.

**Step 1: Apply Symmetry**

Since the deck is randomly shuffled, all five gaps have the same expected length:

E[G₁] = E[G₂] = ... = E[G₅]

**Step 2: Apply Linearity of Expectation**

The total number of non-ace cards is 48:

G₁ + G₂ + G₃ + G₄ + G₅ = 48

Therefore:

E[G₁] = 48/5 = 9.6

**Step 3: Include the First Ace**

The number of draws includes all cards in the first gap plus the first ace.

**E[X] = E[G₁] + 1 = 10.6**

### General Formula

For N cards containing K special cards, the expected number of draws until the first special card is:

**E[X] = (N + 1)/(K + 1)**

For the first ace:

E[X] = 53/5 = 10.6

### Follow-up: Expected Draws Until the r-th Ace

To reach the r-th ace, we pass through r gaps and draw r aces.

E[Xᵣ] = r × (N − K)/(K + 1) + r

Therefore:

**E[Xᵣ] = r(N + 1)/(K + 1)**

For the second ace:

E[X₂] = 2 × 53/5 = 21.2

### Key Takeaways

- **Technique:** Symmetry + Linearity of Expectation
- **Concept:** Order Statistics / Expected Waiting Time
- **General Formula:** E[Xᵣ] = r(N + 1)/(K + 1)
- **Interview Tip:** Use the gap argument instead of calculating the full probability distribution.
- **Important:** Excluding the first ace gives 9.6; including it gives 10.6.

---

[Previous question](question-1.md) · [Day 1 overview](index.md) · [Next question](question-3.md)
