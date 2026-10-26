# Day 1 · Dot Placement

*Edited by Charles*

| Study record | Value |
| --- | --- |
| Question | 4 of 4 |
| Topic | Probability |
| Difficulty | Easy |
| Attempts | 1 |
| Mastery (Notion notation) | ❚ |
| Latest attempt | October 22, 2026 |
| Source | [OpenQuant: Dot Placement](https://openquant.co/questions/dot-placement) |

## Probability of Three Dots on Distinct Edges

### Problem

You place three dots independently and uniformly along the edges of a regular octagon. What is the probability that all three dots lie on distinct edges?

### Solution: Sequential Probability

A regular octagon has **8 edges**, each equally likely to contain a dot.

- **First dot:** Any edge → P₁ = 8/8 = 1
- **Second dot:** Must choose a different edge → P₂ = 7/8
- **Third dot:** Must avoid the previous two edges → P₃ = 6/8

Multiplying the conditional probabilities:

**P = (8/8) × (7/8) × (6/8) = 21/32 = 65.625%**

### Alternative: Combinatorics

- Total outcomes: 8³
- Favorable outcomes: 8 × 7 × 6

**P = (8 × 7 × 6)/8³ = 21/32**

### General Formula

For n equally likely edges and k independently placed dots (k ≤ n):

**P(all distinct) = n! / [(n − k)! × nᵏ]**

### Key Takeaways

- **Technique:** Conditional Probability / Counting
- **Concept:** Sampling with Replacement, Birthday Problem
- **Interview Tip:** Use sequential conditional probabilities for the quickest solution.
- **Assumption:** All edges have equal probability, and dot placements are independent.

---

[Previous question](question-3.md) · [Day 1 overview](index.md)
