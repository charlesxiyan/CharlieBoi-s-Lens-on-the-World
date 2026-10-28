---
hide:
  - toc
---

# 503. Next Greater Element II

*Edited by Charles*

**Medium · Python · Monotonic Stack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/next-greater-element-ii/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given a **circular array** `nums`, return the next greater element for every element.

    The **next greater element** is the first element to the right that is strictly greater than the current element. Since the array is circular, after reaching the end, the search continues from the beginning.

## Python solution

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        l = len(nums)
        res = [-1] * l
        stack = []

        for i in range(l):
            while stack and nums[i] > nums[stack[-1]]:
                res[stack[-1]] = nums[i]
                stack.pop()
            stack.append(i)

        for i in range(l):
            if not stack:
                return res
            while stack and nums[i] > nums[stack[-1]]:
                res[stack[-1]] = nums[i]
                stack.pop()

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given a **circular array** `nums`, return the next greater element for every element.

    The **next greater element** is the first element to the right that is strictly greater than the current element. Since the array is circular, after reaching the end, the search continues from the beginning.

    If no greater element exists, return `-1`.

    For example:

    ```python
    nums = [1,2,1]
    ```

    Output:

    ```python
    [2,-1,2]
    ```

    The key observation is:

    > This problem is essentially the same as **LeetCode 496 (Next Greater Element I)**, except that the array is **circular**.
    >

    A monotonic stack can still solve the problem efficiently. The only difference is that we need to simulate traversing the array twice.

    ## Monotonic Stack

    ### Why Use a Stack?

    For every element, we need to find the **first greater element on its right**.

    A brute-force solution would scan forward (possibly wrapping around) for every element, resulting in **O(n²)** time complexity.

    Using a monotonic stack allows each index to be pushed and popped at most once, resulting in an **O(n)** solution.

    ### What Should Be Stored in the Stack?

    The stack stores **indices** of the array.

    ```python
    stack = [index]
    ```

    Whenever the value is needed, simply access

    ```python
    nums[index]
    ```

    ### What Kind of Monotonic Stack?

    We maintain a **monotonically decreasing stack**.

    That means:

    ```text
    Values decrease from bottom to top.
    ```

    Whenever a larger value appears, it becomes the next greater element for every smaller value on the top of the stack.

    ## How to Handle the Circular Array?

    Unlike LeetCode 496, elements at the end of the array may find their next greater element at the beginning.

    For example:

    ```python
    nums = [1,2,1]
    ```

    The last `1` should find `2` by wrapping around.

    One simple approach is:

    1. Traverse the array once normally.
    2. Traverse it a second time without pushing new indices.

    The first traversal builds the monotonic stack.

    The second traversal only helps the remaining indices in the stack find their next greater element.

    Since every index is already in the stack after the first pass, there is no need to push indices again.

    ## State Transition

    During traversal, there are three situations.

    ### Case 1: Current value is smaller

    If

    ```python
    nums[i] <= nums[stack[-1]]
    ```

    the current value cannot resolve any previous element.

    Push its index into the stack (first traversal only).

    ```python
    stack.append(i)
    ```

    ### Case 2: Current value is greater

    If

    ```python
    nums[i] > nums[stack[-1]]
    ```

    then the current value is the next greater element for the stack top.

    Continue popping while the current value is greater.

    ```python
    while stack and nums[i] > nums[stack[-1]]:
        idx = stack.pop()
        res[idx] = nums[i]
    ```

    After processing all smaller elements, push the current index (first traversal only).

    ### Second Traversal

    The second traversal repeats the comparison from the beginning of the array.

    However, **do not push indices again**.

    It only exists to resolve the indices that remain in the stack after the first traversal.

    ```python
    for i in range(len(nums)):
        while stack and nums[i] > nums[stack[-1]]:
            res[stack.pop()] = nums[i]
    ```

    If the stack becomes empty, every index has already found its next greater element, so we can terminate early.

    ## Example

    ```python
    nums = [1,2,1]
    ```

    Initialization:

    ```text
    Stack = []
    Result = [-1,-1,-1]
    ```

    ### First Traversal

    #### i = 0

    ```text
    Stack = [0]
    ```

    #### i = 1

    ```text
    2 > 1
    ```

    Pop index `0`.

    ```text
    Result[0] = 2
    ```

    Push index `1`.

    ```text
    Stack = [1]
    ```

    #### i = 2

    ```text
    1 < 2
    ```

    Push index `2`.

    ```text
    Stack = [1,2]
    ```

    At the end of the first traversal:

    ```text
    Result = [2,-1,-1]
    Stack  = [1,2]
    ```

    ### Second Traversal

    #### i = 0

    ```text
    1 > 1 ? No
    ```

    Nothing happens.

    #### i = 1

    ```text
    2 > 1
    ```

    Pop index `2`.

    ```text
    Result[2] = 2
    ```

    Now:

    ```text
    2 > 2 ? No
    ```

    Stop.

    The remaining index (`1`) has no greater element, so its answer stays `-1`.

    Final result:

    ```python
    [2,-1,2]
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n)
    ```

    Although the array is traversed twice, each index is pushed once and popped at most once.

    **Space Complexity**

    ```text
    O(n)
    ```

    The stack may contain all indices.

    ## Summary

    **Data Structure**

    ```text
    Monotonic Stack
    ```

    **Stack Content**

    ```text
    Indices of nums
    ```

    **Monotonic Property**

    ```text
    Values decrease from bottom to top.
    ```

    **Transition**

    ```python
    while stack and nums[i] > nums[stack[-1]]:
        res[stack.pop()] = nums[i]
    ```

    **Traversal Order**

    ```text
    First pass: build the stack and resolve next greater elements.
    Second pass: simulate the circular array by resolving the remaining indices.
    ```

    **Final Answer**

    ```text
    The result array stores the next greater element for every index.
    ```

    ## Common Mistakes

    1. Forgetting that the array is **circular**.

        Elements near the end may find their next greater element at the beginning of the array.

    2. During the second traversal, **do not push indices into the stack again**.

        The second traversal only helps unresolved indices find their next greater element.

    3. The comparison should be:

    ```python
    nums[i] > nums[stack[-1]]
    ```

    because the problem asks for the **next strictly greater element**.

    1. Continue popping while the current value is greater.

    ```python
    while stack and nums[i] > nums[stack[-1]]:
    ```

    1. Initialize the answer array with:

    ```python
    [-1] * len(nums)
    ```

    Any index still remaining in the stack after the second traversal has no greater element in the circular array, so its answer should remain `-1`.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 13, 2026 |

---

[Back to Day 41 Monotonic Stack I](index.md) · [All Algorithm notes](../index.md)
