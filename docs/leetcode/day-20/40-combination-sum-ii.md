---
hide:
  - toc
---

# 40. Combination Sum II

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/combination-sum-ii/description/)

!!! tip "Core idea"

    Recall [39. Combination Sum](39-combination-sum.md) for the core idea. The pruning in this solution is: take [1, 1, 2, 5, 6, 7, 10], target = 8 as an example.

    ```python
    i > startIndex and candidates[i] == candidates[i - 1]
    ```

## Python solution

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        candidates.sort()
        self.backtrack(candidates, target, 0, 0, [], result)
        return result

    def backtrack(self, candidates, targetSum, curSum, startIndex, path, result):
        if curSum > targetSum:
            return
        if curSum == targetSum:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            if i > startIndex and candidates[i] == candidates[i - 1]:
                continue
            if curSum + candidates[i] > targetSum:
                break

            curSum += candidates[i]
            path.append(candidates[i])
            self.backtrack(candidates, targetSum, curSum, i + 1, path, result)
            curSum -= candidates[i]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [39. Combination Sum](39-combination-sum.md) for the core idea.
    2. The pruning in this solution is: take [1, 1, 2, 5, 6, 7, 10], target = 8 as an example.
        1. Following the solution in [39. Combination Sum](39-combination-sum.md) will give duplicates of [1, 2, 5] and [1, 7], since paths are twice initiated at 1. Thus, in the beginning of the for-loop, consider such a condition and skip the iteration

            ```python
            i > startIndex and candidates[i] == candidates[i - 1]
            ```

        2. The solution in [39. Combination Sum](39-combination-sum.md) also continue iterating even though the curSum is greater than the target, say [1, 1, 7] and [1, 1, 10]

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 22, 2026 |

---

[Back to Day 20 Backtrack II](index.md) · [All LeetCode notes](../index.md)
