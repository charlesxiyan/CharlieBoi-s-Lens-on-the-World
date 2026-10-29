---
hide:
  - toc
---

# 47. Permutations II

*Edited by Charles*

**Medium · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/permutations-ii/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Recall [46. Permutations](46-permutations.md) for the core idea of the permutation. Recall [491. Non-decreasing Subsequences](491-non-decreasing-subsequences.md) for the core idea of non-duplicate subsets.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [46. Permutations](46-permutations.md) for the core idea of the permutation.
    2. Recall [491. Non-decreasing Subsequences](491-non-decreasing-subsequences.md) for the core idea of non-duplicate subsets.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 25, 2026 |

## Python solution

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtrack(nums, [False] * len(nums), [], result)
        return result

    def backtrack(self, nums, used, path, result):
        if len(path) == len(nums):
            result.append(path[:])
            return

        uset = set()
        for i in range(len(nums)):
            if used[i] or nums[i] in uset:
                continue

            uset.add(nums[i])
            used[i] = True
            path.append(nums[i])
            self.backtrack(nums, used, path, result)
            path.pop()
            used[i] = False
```

---

[Back to Day 22 Backtrack IV](index.md) · [All Algorithm notes](../index.md)
