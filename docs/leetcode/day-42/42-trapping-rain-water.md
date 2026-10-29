---
hide:
  - toc
---

# 42. Trapping Rain Water

*Edited by Charles*

**Hard · Python · Monotonic Stack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/trapping-rain-water/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given an array `height`, where each element represents the height of a bar, compute how much rainwater can be trapped after raining.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given an array `height`, where each element represents the height of a bar, compute how much rainwater can be trapped after raining.

    For example:

    ```python
    height = [0,1,0,2,1,0,1,3,2,1,2,1]
    ```

    Output:

    ```python
    6
    ```

    Although this problem can be solved using prefix maximum arrays or two pointers, it can also be solved elegantly with a **monotonic stack**.

    The key observation is:

    > Whenever we find a bar that is **higher than the current stack top**, a "container" (or valley) is formed, and we can calculate the trapped water immediately.
    >

    Unlike the prefix maximum solution, the monotonic stack computes the trapped water **layer by layer (horizontally)**.

    ## Monotonic Stack

    ### Why Use a Stack?

    For every bar, we need to determine:

    - the first higher bar on its left
    - the first higher bar on its right

    Once both boundaries are known, the trapped water can be computed.

    A monotonic stack helps us find these boundaries in **O(n)** time.

    ### What Should Be Stored in the Stack?

    The stack stores **indices**.

    ```python
    stack = [index]
    ```

    Whenever the height is needed, simply access

    ```python
    height[index]
    ```

    The indices are also required to compute the width.

    ### What Kind of Monotonic Stack?

    We maintain a **monotonically increasing stack of heights**.

    That means:

    ```text
    Heights increase from top to bottom.
    ```

    Equivalently,

    ```text
    Heights decrease from bottom to top.
    ```

    For example,

    ```text
    Top
    1
    3
    5
    Bottom
    ```

    Whenever a higher bar appears, it closes one or more valleys.

    ## How Is Water Computed?

    Suppose we pop one element from the stack.

    ```text
    Left Wall        Right Wall
         │               │
         │               │
         │      █        │
         │      █        │
    ██████      ██████████
    ```

    The popped element is the **bottom** of the valley.

    After popping:

    - the new stack top is the **left wall**
    - the current index is the **right wall**

    Therefore:

    ```text
    Water Height
    =
    min(left wall, right wall)
    -
    bottom height
    ```

    ```python
    water_height = min(height[left], height[right]) - height[mid]
    ```

    The width is

    ```text
    right - left - 1
    ```

    So the trapped water is

    ```python
    water_height * width
    ```

    ## State Transition

    During traversal, there are three situations.

    ### Case 1: Current bar is lower

    If

    ```python
    height[i] < height[stack[-1]]
    ```

    the current bar cannot close any valley.

    Simply push its index.

    ```python
    stack.append(i)
    ```

    ### Case 2: Current bar has the same height

    If

    ```python
    height[i] == height[stack[-1]]
    ```

    the current implementation simply pushes the new index.

    ```python
    stack.append(i)
    ```

    Some implementations replace the previous index with the current one because using the rightmost bar simplifies width calculation. Both approaches can produce correct results with the appropriate implementation.

    ### Case 3: Current bar is higher

    If

    ```python
    height[i] > height[stack[-1]]
    ```

    then one or more valleys are completed.

    Continue popping while the current bar is taller.

    ```python
    while stack and height[i] > height[stack[-1]]:
    ```

    The popped element becomes the valley bottom.

    ```python
    mid = stack.pop()
    ```

    If the stack is empty after popping, there is no left boundary.

    Otherwise,

    ```python
    left = stack[-1]
    ```

    Compute the trapped water.

    Height:

    ```python
    h = min(height[left], height[i]) - height[mid]
    ```

    Width:

    ```python
    w = i - left - 1
    ```

    Area:

    ```python
    res += h * w
    ```

    Finally,

    push the current index.

    ```python
    stack.append(i)
    ```

    ## Example

    ```python
    height = [4,2,0,3,2,5]
    ```

    Initialization:

    ```text
    Stack = [0]
    Water = 0
    ```

    ### i = 1

    ```text
    2 < 4
    ```

    Push.

    ```text
    Stack = [0,1]
    ```

    ### i = 2

    ```text
    0 < 2
    ```

    Push.

    ```text
    Stack = [0,1,2]
    ```

    ### i = 3

    ```text
    3 > 0
    ```

    Pop:

    ```text
    mid = 2
    ```

    Left boundary:

    ```text
    index = 1
    height = 2
    ```

    Right boundary:

    ```text
    index = 3
    height = 3
    ```

    Water:

    ```text
    height = min(2,3)-0 = 2
    width  = 3-1-1 = 1
    area   = 2
    ```

    Continue.

    ```text
    3 > 2
    ```

    Pop again.

    ```text
    mid = 1
    ```

    Left boundary:

    ```text
    height = 4
    ```

    Right boundary:

    ```text
    height = 3
    ```

    Water:

    ```text
    height = min(4,3)-2 = 1
    width  = 3-0-1 = 2
    area   = 2
    ```

    Current water:

    ```text
    4
    ```

    Push index `3`.

    Continue until the traversal finishes.

    Final answer:

    ```python
    9
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n)
    ```

    Each index is pushed and popped at most once.

    **Space Complexity**

    ```text
    O(n)
    ```

    The stack may contain all indices in the worst case.

    ## Summary

    **Data Structure**

    ```text
    Monotonic Stack
    ```

    **Stack Content**

    ```text
    Indices of bars
    ```

    **Monotonic Property**

    ```text
    Heights decrease from bottom to top.
    ```

    **Water Formula**

    ```python
    height = min(left_height, right_height) - bottom_height
    width = right - left - 1
    water = height * width
    ```

    **Traversal Order**

    ```text
    Left to right
    ```

    **Final Answer**

    ```text
    Accumulate the trapped water for every completed valley.
    ```

    ## Common Mistakes

    - Store **indices**, not heights. The indices are needed to compute the width.
    - After popping the valley bottom, always check whether the stack is empty. If it is, there is no left boundary, so no water can be trapped.
    - The trapped water height is

    ```python
    min(left_height, right_height) - bottom_height
    ```

    not

    ```python
    max(left_height, right_height) - bottom_height
    ```

    - The width is

    ```python
    right - left - 1
    ```

    not

    ```python
    right - left
    ```

    because the two boundary bars are not included.

    - Continue popping while the current bar is higher than the stack top.

    ```python
    while stack and height[i] > height[stack[-1]]:
    ```

    A single higher bar may complete multiple valleys.

    - This problem computes trapped water **horizontally**, one valley at a time, rather than calculating the water above each bar individually.

    ## Why Can the Three Cases Be Simplified?

    Similar to **LeetCode 739 (Daily Temperatures)**, we can initially analyze three situations when processing a new bar.

    ### Case 1: Current bar is lower

    ```python
    height[i] < height[stack[-1]]
    ```

    The current bar cannot close any valley.

    Simply push it into the stack.

    ### Case 2: Current bar has the same height

    ```python
    height[i] == height[stack[-1]]
    ```

    Some implementations replace the previous index with the current one, because using the rightmost bar may simplify width calculation.

    However, simply pushing the new index also works correctly.

    ### Case 3: Current bar is higher

    ```python
    height[i] > height[stack[-1]]
    ```

    A valley is completed.

    The stack top becomes the valley bottom, and the current bar becomes the right boundary.

    We repeatedly pop the stack and calculate trapped water.

    ### Why Can We Merge the Three Cases?

    Notice that Cases 1 and 2 actually perform the **same action**:

    ```python
    stack.append(i)
    ```

    Neither case computes any trapped water.

    Only **Case 3** performs additional work by repeatedly popping the stack.

    Therefore, instead of writing

    ```python
    if height[i] < height[stack[-1]]:
        stack.append(i)
    elif height[i] == height[stack[-1]]:
        stack.append(i)
    else:
        while ...
    ```

    we can directly write

    ```python
    while stack and height[i] > height[stack[-1]]:
        ...
    stack.append(i)
    ```

    This works because:

    - If the current bar is lower, the `while` condition is false, so we simply push it.
    - If the current bar is equal, the `while` condition is also false, so we simply push it.
    - Only when the current bar is higher does the `while` loop execute and compute trapped water.

    As a result, the three explicit cases naturally collapse into a much cleaner implementation.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 15, 2026 |

## Python solution

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        stack = [0]
        res = 0

        for i in range(1, len(height)):
            while len(stack) != 0 and height[stack[-1]] < height[i]:
                mid = height[stack.pop()]
                if stack:
                    h = min(height[stack[-1]], height[i]) - mid
                    w = i - stack[-1] - 1
                    res += h * w
            stack.append(i)

        return res
```

---

[Back to Day 42 Monotonic Stack II](index.md) · [All Algorithm notes](../index.md)
