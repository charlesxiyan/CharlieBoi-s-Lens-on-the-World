---
hide:
  - toc
---

# 90. Subsets II

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/subsets-ii/description/)

!!! tip "Core idea"

    Recall [40. Combination Sum II](../day-20/40-combination-sum-ii.md) and [78. Subsets](78-subsets.md) for the core ideas. The below condition

    ```python
    i > startIndex and nums[i] == nums[i - 1]:
    ```

## Python solution

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        self.backtrack(nums, 0, [], result)
        return result

    def backtrack(self, nums, startIndex, path, result):
        result.append(path[:])

        for i in range(startIndex, len(nums)):
            if i > startIndex and nums[i] == nums[i - 1]:
                continue

            path.append(nums[i])
            self.backtrack(nums, i + 1, path, result)
            path.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [40. Combination Sum II](../day-20/40-combination-sum-ii.md) and [78. Subsets](78-subsets.md) for the core ideas.
    2. The below condition

        ```python
        i > startIndex and nums[i] == nums[i - 1]:
        ```

        ensures, within the for loop, duplicates are checked, for example, [1, 2, 2] with startIndex = 0, the current possible paths are [1, 2], [1, 2], if we do not consider the deeper recurrence with parsing i + 1 to startIndex.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 24, 2026 |

---

[Back to Day 21 Backtrack III](index.md) · [All LeetCode notes](../index.md)
