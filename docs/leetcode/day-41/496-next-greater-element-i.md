---
hide:
  - toc
---

# 496. Next Greater Element I

*Edited by Charles*

**Easy · Python · Monotonic Stack**

[Original problem](https://leetcode.com/problems/next-greater-element-i/description/)

!!! tip "Core idea"

    You are given two arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

    For each element in `nums1`, find its **next greater element** in `nums2`.

## Python solution

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        l1 = len(nums1)
        l2 = len(nums2)

        res = [-1] * l1
        stack = [0]

        for i in range(1, l2):
            if nums2[i] <= nums2[stack[-1]]:
                stack.append(i)
            else:
                while len(stack) != 0 and nums2[i] > nums2[stack[-1]]:
                    if nums2[stack[-1]] in nums1:
                        idx = nums1.index(nums2[stack[-1]])
                        res[idx] = nums2[i]
                    stack.pop()
                stack.append(i)
        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    You are given two arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

    For each element in `nums1`, find its **next greater element** in `nums2`.

    The **next greater element** is the first element to the right that is strictly larger. If no such element exists, return `-1`.

    For example:

    ```python
    nums1 = [4,1,2]
    nums2 = [1,3,4,2]
    ```

    Output:

    ```python
    [-1,3,-1]
    ```

    The key observation is:

    > We only need to find the **first greater element on the right** for every element in `nums2`.
    >

    This is a classic application of a **monotonic stack**.

    ```python
    class Solution:
        def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
    				l1, l2 = len(nums1), len(nums2)
    				stack = [0]

    				for i in range()
    ```

    ## Monotonic Stack

    ### Why Use a Stack?

    For every element in `nums2`, we want to know the first greater element to its right.

    A brute-force solution scans to the right for every element, resulting in **O(n²)** time complexity.

    Instead, we maintain a monotonic stack so that each element is pushed and popped at most once, reducing the complexity to **O(n)** for processing `nums2`.

    ### What Should Be Stored in the Stack?

    The stack stores **indices** of elements in `nums2`.

    ```python
    stack = [index]
    ```

    Whenever the value is needed, simply access

    ```python
    nums2[index]
    ```

    ### What Kind of Monotonic Stack?

    Since we are searching for the **next greater element**, we maintain a **monotonically decreasing stack**.

    That means:

    ```text
    Values decrease from bottom to top.
    ```

    For example,

    ```text
    Bottom
    9
    7
    5
    3
    Top
    ```

    Whenever a larger value appears, it becomes the next greater element for all smaller values on the top of the stack.

    ## State Transition

    During traversal of `nums2`, there are three situations.

    ### Case 1: Current value is smaller

    If

    ```python
    nums2[i] < nums2[stack[-1]]
    ```

    the current value cannot be the next greater element for any previous number.

    Push its index into the stack.

    ```python
    stack.append(i)
    ```

    ### Case 2: Current value is equal

    If

    ```python
    nums2[i] == nums2[stack[-1]]
    ```

    push it into the stack as well.

    The problem asks for the **next strictly greater element**, so equal values cannot resolve previous elements.

    ```python
    stack.append(i)
    ```

    ### Case 3: Current value is greater

    If

    ```python
    nums2[i] > nums2[stack[-1]]
    ```

    then the current value is the next greater element for the stack top.

    Continue popping while the current value is larger.

    ```python
    while stack and nums2[i] > nums2[stack[-1]]:
    ```

    For every popped index:

    - Check whether the corresponding value exists in `nums1`.
    - If it does, update the answer.

    ```python
    idx = nums1.index(nums2[stack[-1]])
    res[idx] = nums2[i]
    ```

    After all smaller elements are processed,

    push the current index.

    ```python
    stack.append(i)
    ```

    ## Example

    ```python
    nums1 = [2,4]
    nums2 = [1,2,3,4]
    ```

    Initialization:

    ```text
    Stack = [0]      # value = 1
    Result = [-1,-1]
    ```

    ### i = 1 (value = 2)

    ```text
    2 > 1
    ```

    Pop index `0`.

    Since `1` is not in `nums1`, ignore it.

    Push index `1`.

    ```text
    Stack = [1]
    ```

    ### i = 2 (value = 3)

    ```text
    3 > 2
    ```

    Pop index `1`.

    Since `2` is in `nums1`:

    ```text
    Result[0] = 3
    ```

    Push index `2`.

    ```text
    Stack = [2]
    ```

    ### i = 3 (value = 4)

    ```text
    4 > 3
    ```

    Pop index `2`.

    `3` is not in `nums1`.

    Push index `3`.

    ```text
    Stack = [3]
    ```

    Traversal finishes.

    The remaining element (`4`) has no greater element on its right, so its answer remains `-1`.

    Final result:

    ```python
    [3,-1]
    ```

    ## Complexity Analysis

    ### Current Implementation

    **Time Complexity**

    ```text
    O(n × m)
    ```

    where

    - `n = len(nums2)`
    - `m = len(nums1)`

    Although the monotonic stack itself is **O(n)**, the following operations are both linear:

    ```python
    nums2[stack[-1]] in nums1
    nums1.index(...)
    ```

    Each pop may perform an `O(m)` search.

    **Space Complexity**

    ```text
    O(n)
    ```

    The stack may contain all indices.

    ### Optimization

    A better approach is to build a hash map:

    ```python
    value -> index in nums1
    ```

    For example:

    ```python
    index_map = {
        value: i
        for i, value in enumerate(nums1)
    }
    ```

    Then both lookup operations become **O(1)**:

    ```python
    if nums2[idx] in index_map:
        res[index_map[nums2[idx]]] = nums2[i]
    ```

    Overall complexity becomes:

    ```text
    Time:  O(n)
    Space: O(n)
    ```

    ## Summary

    **Data Structure**

    ```text
    Monotonic Stack
    ```

    **Stack Content**

    ```text
    Indices of nums2
    ```

    **Monotonic Property**

    ```text
    Values decrease from bottom to top.
    ```

    **Transition**

    ```python
    while stack and nums2[i] > nums2[stack[-1]]:
        ...
    stack.append(i)
    ```

    **Traversal Order**

    ```text
    Left to right
    ```

    **Final Answer**

    ```text
    The result array stores the next greater element for every value in nums1.
    ```

    ## Common Mistakes

    1. Store **indices**, not values.
    2. The comparison should be:

    ```python
    nums2[i] > nums2[stack[-1]]
    ```

    because we need the **next strictly greater element**.

    1. Continue popping while the current value is larger.

    ```python
    while stack and nums2[i] > nums2[stack[-1]]:
    ```

    1. The remaining elements in the stack have no greater element on the right, so their answers remain -1
    2. **Performance Improvement**

    The current implementation uses:

    ```python
    nums2[stack[-1]] in nums1
    nums1.index(...)
    ```

    Both operations are **O(m)**.

    Replacing them with a hash map reduces the overall complexity from **O(n × m)** to **O(n)**.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 13, 2026 |

---

[Back to Day 41 Monotonic Stack I](index.md) · [All LeetCode notes](../index.md)
