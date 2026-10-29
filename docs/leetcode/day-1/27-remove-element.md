---
hide:
  - toc
---

# 27. Remove Element

*Edited by Charles*

**Easy · Python · Array**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/remove-element/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        **Dual Pointers from the Start**: Consider using two pointers that both start from the beginning of the array. **Element Overwriting**: The goal is to keep only the elements that are not equal to `val`. This means we are not "swapping" elements, but rather "overwriting" them. Elements equal to `val` will be covered (overwritten) by subsequent elements that are not equal to `val`.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ### **LeetCode 27. Remove Element**

    1. **Dual Pointers from the Start**: Consider using two pointers that both start from the beginning of the array.
    2. **Element Overwriting**: The goal is to keep only the elements that are not equal to `val`. This means we are not "swapping" elements, but rather "overwriting" them. Elements equal to `val` will be covered (overwritten) by subsequent elements that are not equal to `val`.
    3. **Pointer Roles**:
        - **3.1 Fast Pointer (`f`)**: Moves forward at its own pace using a `for` loop.
        - **3.2 Slow Pointer (`s`)**: Has a specific function: whenever the **fast pointer** points to an element that is **not** `val`, the slow pointer overwrites its current position with that element and then moves one step forward.
            - **(1)** When the fast and slow pointers are synchronized, the overwriting simply replaces the value with itself (Splendid!).
            - **(2)** When the fast pointer encounters `val`, the slow pointer stays put. This is because the slow pointer is now pointing at an element that needs to be overwritten. The fast pointer continues to move forward to find the next element that is not `val`.
    4. **Result Interpretation**: Since Python uses **0-based indexing**, the final value of the slow pointer represents the total count of elements not equal to `val`. These elements have been stored in the array from index `0` to `s-1`.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 4 |
    | Mastery | ❚❚❚❚ |
    | The Latest Attempt | December 31, 2025 |
    | Feedback | 一开始想到了双指针，但是总是想前后双指针解决问题，同时起步的双指针要多练习！ |

## Python solution

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        n = len(nums)
        s = 0

        for f in range(n):
            # nums[i] == val
            #   pass and let record stay
            # nums[i] != val
            #   replace and let record iterate
            if nums[f] != val:
                nums[s] = nums[f]
                s += 1
        return s
```

---

[Back to Day 1 Array I](index.md) · [All Algorithm notes](../index.md)
