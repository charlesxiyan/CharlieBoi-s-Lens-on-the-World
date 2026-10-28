---
hide:
  - toc
---

# 349. Intersection of Two Arrays

*Edited by Charles*

**Easy · Python · HashTable**

[Original problem](https://leetcode.com/problems/intersection-of-two-arrays/description/)

!!! tip "Core idea"

    Create two sets of distinctive numbers in nums1 and nums2 respectively. Initialize the result list.

## Python solution

```python
# List
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        set_1 = set(nums1)
        set_2 = set(nums2)
        res = []

        for num in nums1:
            if num in nums2 and num not in res:
                res.append(num)

        return res

# Set
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        set_1 = set(nums1)
        set_2 = set(nums2)
        res = set()

        for num in nums1:
            if num in nums2:
                res.add(num)

        return list(res)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. Create two sets of distinctive numbers in nums1 and nums2 respectively.
    2. Initialize the result list.
    3. Iterate one array and check if the current element is in the set created by nums2, meanwhile check if the element is in the result list (no duplicate).
    4. Since the result is initialized in the form of a list, we need to check duplicate values. If we initialize the result in the form of a set, we do not need to worry about duplication.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 5, 2026 |

---

[Back to Day 5 Hash Table I](index.md) · [All Algorithm notes](../index.md)
