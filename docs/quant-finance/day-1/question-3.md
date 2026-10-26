# Day 1 · Highest Roll EV

*Edited by Sophia*

| Study record | Value |
| --- | --- |
| Question | 3 of 4 |
| Topic | Probability |
| Difficulty | Medium |
| Attempts | 1 |
| Mastery (Notion notation) | ❚ |
| Latest attempt | October 22, 2026 |
| Source | [OpenQuant: Highest Roll EV](https://openquant.co/questions/highest-roll-ev) |

## Problem

Jim will roll a fair, six-sided die until he gets a 4. What is the expected value of the highest number he rolls through this process?

## Solution

Since the game stops as soon as a 4 is rolled, 4 is always rolled at least once, meaning the maximum value is at least 4. Any rolls of 1, 2, or 3 have no impact on the maximum.
Therefore, the problem simplifies entirely to the relative arrival order of the numbers **4, 5, and 6**.
By symmetry, all $3! = 6$ relative orderings of $\{4, 5, 6\}$ are equally likely:

- If 4 appears before both 5 and 6 (orders: `4-5-6`, `4-6-5` $\to 2$ outcomes), neither 5 nor 6 appears before the game ends, so the maximum is **4**.

- If 5 appears before 4, but 6 appears after 4 (order: `5-4-6` $\to 1$ outcome), a 5 was rolled but no 6 was rolled, so the maximum is **5**.

- If 6 appears before 4 (orders: `6-4-5`, `6-5-4`, `5-6-4` $\to 3$ outcomes), a 6 was rolled, so the maximum is **6**.
Taking the expectation across these 6 outcomes: 31/6.

---

[Previous question](question-2.md) · [Day 1 overview](index.md) · [Next question](question-4.md)
