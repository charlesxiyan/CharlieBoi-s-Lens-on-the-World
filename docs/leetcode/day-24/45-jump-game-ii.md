---
hide:
  - toc
---

# 45. Jump Game II

*Edited by Charles*

**Medium · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/jump-game-ii/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recall [55. Jump Game](55-jump-game.md) for the core idea of the maximum coverage. Take nums = [3, 2, 1, 1, 2] as an example.

    i = 0: far = 0 → 3, base = 0 → 3, steps = 0 → 1 since the current maximum coverage is 0 = 0. i = 1: far = 3, base = 3, unchanged since the current maximum coverage is 3 ≠ 1.

## Python solution

```python
class Solution:
    def jump(self, nums: List[int]) -> int:

        l = r = 0
        n = len(nums)
        res = 0
        fur = 0
        if n == 1:
            return 0
        while True:
            for i in range(l,r+1):
                fur = max(fur,nums[i] + i)
            # 更新下一轮循环的区间
            l = r + 1
            r = fur
            # 计数res
            res += 1
            if r >= n - 1:
                return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    Recall [55. Jump Game](55-jump-game.md) for the core idea of the maximum coverage. Take nums = [3, 2, 1, 1, 2] as an example.

    1. i = 0: far = 0 → 3, base = 0 → 3, steps = 0 → 1 since the current maximum coverage is 0 = 0.
    2. i = 1: far = 3, base = 3, unchanged since the current maximum coverage is 3 ≠ 1.
    3. i = 2: far = 3, base = 3, unchanged 3 ≠ 2
    4. i = 3: far = 3 → 4, base = 3 → 4, steps = 1 → 2 since the current maximum coverage is 3 = 3.
        1. nums[3] = 1 ≠ 0 fundamentally solves the problem of the last step since far is updated and there is only one step to the end of the list.
        2. It has already been announced that it is guaranteed to reach the end of the list, and thus the last element is not required to be iterated.

    ```python
    class Solution:
        def jump(self, nums: List[int]) -> int:
            l = len(nums)
            # base refers to the current maximum coverage without
            # taking nums[i] into account
            base = 0
            # far refers to the current maximum coverage if
            # taking nums[i] into account
            far = 0
            steps = 0

            for i in range(l - 1):
                far = max(far, nums[i] + i)
                if i == base:
                    base = far
                    steps += 1

            return steps
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 27, 2026 |

---

[Back to Day 24 Greedy Algorithm II](index.md) · [All Algorithm notes](../index.md)
