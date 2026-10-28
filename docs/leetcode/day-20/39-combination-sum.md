---
hide:
  - toc
---

# 39. Combination Sum

*Edited by Charles*

**Medium · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/combination-sum/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recall [216. Combination Sum III](../day-19/216-combination-sum-iii.md) for the core idea of the backtrack algorithm. The most fabulous part in this solution is multiple usage of the same number is permitted, and thus, in each recurrence, there is no need to parse i + 1 to the startIndex, and we should allow to parse i to the startIndex, instead.

## Python solution

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        self.backtrack(candidates, target, 0, 0, [], result)
        return result

    def backtrack(self, candidates, targetSum, startIndex, curSum, path, result):
        if targetSum < curSum:
            return

        if targetSum == curSum:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            curSum += candidates[i]
            path.append(candidates[i])
            self.backtrack(candidates, targetSum, i, curSum, path, result)
            curSum -= candidates[i]
            path.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [216. Combination Sum III](../day-19/216-combination-sum-iii.md) for the core idea of the backtrack algorithm.
    2. The most fabulous part in this solution is multiple usage of the same number is permitted, and thus, in each recurrence, there is no need to parse i + 1 to the startIndex, and we should allow to parse i to the startIndex, instead.
    3. Additionally, recall [15. 3Sum](../day-6/15-3sum.md) for a pruning.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 22, 2026 |

---

[Back to Day 20 Backtrack II](index.md) · [All Algorithm notes](../index.md)
