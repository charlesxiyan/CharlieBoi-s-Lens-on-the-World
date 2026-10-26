# Day 1 · Even Before Odd

*Edited by Sophia*

| Study record | Value |
| --- | --- |
| Question | 1 of 4 |
| Topic | Probability |
| Difficulty | Easy |
| Attempts | 1 |
| Mastery (Notion notation) | ❚ |
| Latest attempt | October 22, 2026 |
| Source | [OpenQuant: Even Before Odd](https://openquant.co/questions/even-before-odd) |

## Problem

Suppose you roll a fair 6-sided die until you've seen all 6 faces. What is the probability you won't see an odd numbered face until you have seen all even numbered faces?

## Solution

The key insight is that repeated rolls don't matter—we only need to track the order of the **first appearance** of each distinct face.
By symmetry, all $6!$ permutations of the 6 faces appearing for the first time are equally likely.
The problem requires that all three even faces appear before any odd face shows up. That means the first 3 unique faces must be even, and the remaining 3 must be odd.
There are $3!$ ways to order the even numbers and $3!$ ways to order the odd numbers, giving $3! \times 3!$ valid sequences.
Therefore, the probability is: 1/20.

---

[Day 1 overview](index.md) · [Next question](question-2.md)
