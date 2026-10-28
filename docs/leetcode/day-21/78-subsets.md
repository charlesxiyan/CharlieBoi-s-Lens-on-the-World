---
hide:
  - toc
---

# 78. Subsets

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/subsets/description/)

!!! tip "Core idea"

    the solution does not necessarily need a terminal condition, since the for loop stops at the last element of nums. path is initiated as [].

    ```python
    result.append(path[:])
    ```

## Python solution

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtrack(nums, 0, [], result)
        return result

    def backtrack(self, nums, startIndex, path, result):
        # This step ensures [] and nums itself are appended to the result.
        result.append(path[:])

        for i in range(startIndex, len(nums)):
            path.append(nums[i])
            self.backtrack(nums, i + 1, path, result)
            path.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. the solution does not necessarily need a terminal condition, since the for loop stops at the last element of nums.
    2. path is initiated as [].

        ```python
        result.append(path[:])
        ```

        ensures [] and nums itself are appended to result.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 24, 2026 |

---

[Back to Day 21 Backtrack III](index.md) · [All LeetCode notes](../index.md)
