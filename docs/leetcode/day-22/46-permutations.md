---
hide:
  - toc
---

# 46. Permutations

*Edited by Charles*

**Medium · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/permutations/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Since the length of path is always the length of nums, the for loop in the backtrack function needs to always iterate from 0 to len(nums) - 1. Take [1, 2, 3] as an example.

## Python solution

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtrack(nums, [False] * len(nums), [], result)
        return result

    def backtrack(self, nums, used, path, result):
        if len(path) == len(nums):
            result.append(path[:])
            return

        for i in range(len(nums)):
            if used[i]:
                continue

            used[i] = True
            path.append(nums[i])
            self.backtrack(nums, used, path, result)
            path.pop()
            used[i] = False
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Since the length of path is always the length of nums, the for loop in the backtrack function needs to always iterate from 0 to len(nums) - 1.
    2. Take [1, 2, 3] as an example.
    3. The 1st level recurrence generates [T, F, F] - [1], [F, T, F] - [2] and [F, F, T] - [3].
    4. There are three 2nd level recurrences, which generate [T, T, F] - [1, 2], [T, F, T] - [1, 3]; [T, T, F] - [2, 1], [F, T, T] - [2, 3]; [T, F, T] - [3, 1], [F, T, T] - [3, 2].
    5. Then go to next level recurrences.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 24, 2026 |

---

[Back to Day 22 Backtrack IV](index.md) · [All Algorithm notes](../index.md)
