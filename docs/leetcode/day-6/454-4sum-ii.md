---
hide:
  - toc
---

# 454. 4Sum II

*Edited by Charles*

**Medium · Python · HashTable**

[Original problem](https://leetcode.com/problems/4sum-ii/description/)

!!! tip "Core idea"

    The idea of the solution is counting how many times/combinations num1 (in nums1) + num2 (in nums2) pops up and save them in a dictionary.

    Then iterate all combinations of num3 and num4 in nums3 and nums4. If the opposite number exists in the dictionary, let count be added by the combination number in the dictionary.

## Python solution

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        dict = {}
        count = 0

        for num1 in nums1:
            for num2 in nums2:
                sum_12 = num1 + num2
                if sum_12 not in dict:
                    dict[sum_12] = 1
                else:
                    dict[sum_12] += 1

        for num3 in nums3:
            for num4 in nums4:
                if -num3-num4 in dict:
                    count += dict[-num3-num4]

        return count
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    The idea of the solution is counting how many times/combinations num1 (in nums1) + num2 (in nums2) pops up and save them in a dictionary.

    Then iterate all combinations of num3 and num4 in nums3 and nums4. If the opposite number exists in the dictionary, let count be added by the combination number in the dictionary.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 5, 2026 |

---

[Back to Day 6 Hash Table II](index.md) · [All LeetCode notes](../index.md)
