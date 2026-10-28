---
hide:
  - toc
---

# 1. Two Sum

*Edited by Charles*

**Easy · Python · HashTable**

[Original problem](https://leetcode.com/problems/two-sum/)

!!! tip "Core idea"

    The solution is quite tricky!

    **Note** that the question already assume that each input would have exactly one solution, and there is no duplicate.

## Python solution

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dict = {}
        n = len(nums)

        for i in range(n):
            if nums[i] not in dict:
                dict[target - nums[i]] = i
            else:
                return [dict[nums[i]], i]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    The solution is quite tricky!

    **Note** that the question already assume that each input would have exactly one solution, and there is no duplicate.

    The question requires to return a pair of indices whose values’ sum is the given target.

    1. Create an empty dictionary.
    2. Iterate the array:
        1. if the current value is not in the dictionary, then there must be a value equal to (target - the current value) later in the array.
        2. in order to check this value (target - the current value) easily, we save this value as the key, but its value as the index of the current value, since the return only requires the indices.
        3. once (target - the current value) pops up, we can easily find this value is already in the dictionary. Now, return the key’s value (this index must be less than the current index) and the current index.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 4 |
    | Mastery | ❚❚❚❚ |
    | The Latest Attempt | January 5, 2026 |

---

[Back to Day 5 Hash Table I](index.md) · [All Algorithm notes](../index.md)
