---
hide:
  - toc
---

# 135. Candy

*Edited by Charles*

**Hard · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/candy/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    In order to follow the rules that the child with strictly higher rating must have more candies than either left or right child, we iterate the rating list from left to right once and from right to left once. For the first iteration (left → right), focus on the right child of the current child. If the right rating is strictly higher, give the number of candies that the left child holds plus one to the right child.

## Python solution

```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        result = [1] * n

        for i in range(1, n):
            if ratings[i] > ratings[i - 1]:
                result[i] = result [i - 1] + 1

        for i in range(n - 2, -1, -1):
            if ratings[i + 1] < ratings[i]:
                result[i] = max(result[i + 1] + 1, result[i])

        return sum(result)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. In order to follow the rules that the child with strictly higher rating must have more candies than either left or right child, we iterate the rating list from left to right once and from right to left once.
    2. For the first iteration (left → right), focus on the right child of the current child. If the right rating is strictly higher, give the number of candies that the left child holds plus one to the right child.
    3. For the second iteration (right → left), focus on the left child of the current child. This time, we cannot simply use the same way in 2 to update the candy number. Take candies = […. 4, 1] and ratings = […. 3, 2] as an example after the first iteration. The second last child should keep the candy number 4 instead of 2, although ratings[left] > ratings[right].

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 28, 2026 |

---

[Back to Day 25 Greedy Algorithm III](index.md) · [All Algorithm notes](../index.md)
