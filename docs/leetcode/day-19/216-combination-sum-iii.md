---
hide:
  - toc
---

# 216. Combination Sum III

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/combination-sum-iii/description/)

!!! tip "Core idea"

    Backtrack over digits 1 through 9, increasing the starting digit after each choice. Keep only combinations with exactly `k` numbers and the required sum.

## Python solution

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        result = []
        self.backtrack(n, k, 0, 1, [], result)
        return result

    def backtrack(self, targetSum, k, curSum, startIndex, path, result):
        # Pruning
        if curSum > targetSum:
            return

        # Terminal Condition
        if len(path) == k:
            if targetSum == curSum:
                result.append(path[:])
            return

        for i in range(startIndex, 9 + 1 - (k - len(path)) + 1):
            curSum += i
            path.append(i)
            self.backtrack(targetSum, k, curSum, i + 1, path, result)
            # Backtrack
            curSum -= i
            path.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recall [77. Combinations](77-combinations.md) for backtracking 3 steps.

    The backtrack in this solution will reflect on two variables:

    1. path
    2. current summation

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 21, 2026 |

---

[Back to Day 19 Backtrack I](index.md) · [All Algorithm notes](../index.md)
