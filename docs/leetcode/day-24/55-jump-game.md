---
hide:
  - toc
---

# 55. Jump Game

*Edited by Charles*

**Medium · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/jump-game/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    We mainly focus on the maximum coverage where we can jump. Take nums = [2, 3, 1, 1, 4] as an example (Solution 2).

## Python solution

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        l = len(nums)
        far = nums[0]

        for i in range(l):
            if i > far:
                return False
            far = max(nums[i] + i, far)
            if far >= l - 1:
			          return True

###############################################################

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        l = len(nums)
        i = 0
        idx = 0
        while i <= idx:
            idx = max(nums[i] + i, idx)
            if idx >= l - 1:
                return True
            i += 1

        return False
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. We mainly focus on the maximum coverage where we can jump.
    2. Take nums = [2, 3, 1, 1, 4] as an example (Solution 2).
        1. i = 0: we can maximum jump to idx = 0 + 2 = 2 < 4, and thus cannot determine if we can jump to the end of the list. Move to next element.
        2. i = 1: we can maximum jump to idx = max(2, 1 + 3) = 4. Hence, we can jump to the end of the list.
        3. What we did was to update the maximum coverage by iteration of all elements in the given list. Once we find the maximum coverage exceeds the index of the last element, we stop and return True.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 27, 2026 |

---

[Back to Day 24 Greedy Algorithm II](index.md) · [All Algorithm notes](../index.md)
