---
hide:
  - toc
---

# 376. Wiggle Subsequence

*Edited by Charles*

**Medium · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/wiggle-subsequence/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        If the input has only length 1, then return 1 by default. Consider cases where the current number is equal to the previous number. This could happen in the halfway of going upwards/downwards, and could also happen between a going upwards and going downwards. For example.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. If the input has only length 1, then return 1 by default.
    2. Consider cases where the current number is equal to the previous number. This could happen in the halfway of going upwards/downwards, and could also happen between a going upwards and going downwards. For example.
        1. [1, 2, 2, 2, 1]
        2. [1, 2, 2, 3, 4] or [3, 2, 2, 1]
    3. How can we detect the differences alternate the signs?
        1. Based on nums[i], record prediff and curdiff as the signs of last difference and the incoming difference.
        2. Initiate prediff = 0.
        3. In the for loop from 0 to n - 1, calculate curdiff in each iteration.
            1. Let prediff reflect the case of equality.
            2. When prediff ≥ 0 and curdiff < 0, or when prediff ≤ 0 and curdiff > 0, we count the alternating.
            3. This incorporates the case of length 2. By default, the result if 2 for length-2 case.
            4. As long as the alternating condition is met, we update prediff by curdiff, since we need to avoid such a case, [1, 2, 2, 3, 4]. If we do not use the current updating way, when the second 2 is iterated, prediff = 0, and curdiff = 1, counted as an alternation, and however it is not.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 26, 2026 |

## Python solution

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        n = len(nums)

        if n == 1:
            return n

        prediff = 0
        result = 1

        for i in range(n - 1):
            curdiff = nums[i + 1] - nums[i]

            if (prediff >= 0 and curdiff < 0) or (prediff <= 0  and curdiff > 0):
                result += 1
                prediff = curdiff

        return result
```

---

[Back to Day 23 Greedy Algorithm I](index.md) · [All Algorithm notes](../index.md)
