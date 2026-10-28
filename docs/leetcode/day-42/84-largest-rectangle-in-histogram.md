---
hide:
  - toc
---

# 84. Largest Rectangle in Histogram

*Edited by Charles*

**Hard · Python · Monotonic Stack**

[Original problem](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

!!! tip "Core idea"

    Given an array `heights`, where each element represents the height of a histogram bar, return the area of the largest rectangle that can be formed.

## Python solution

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights.insert(0, 0)
        heights.append(0)
        stack = [0]
        res = 0

        for i in range(1, len(heights)):
            while stack and heights[i] < heights[stack[-1]]:
                mid = heights[stack.pop()]
                if stack:
                    res = max(res, (i - stack[-1] - 1) * mid)
            stack.append(i)

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given an array `heights`, where each element represents the height of a histogram bar, return the area of the largest rectangle that can be formed.

    For example:

    ```python
    heights = [2,1,5,6,2,3]
    ```

    Output:

    ```python
    10
    ```

    The key observation is:

    > For every bar, assume **this bar is the shortest bar in the rectangle**.
    >
    >
    > Then, the only thing we need to determine is **how far this height can extend to the left and right**.
    >

    A monotonic stack allows us to determine the maximum width for every bar in **O(n)** time.

    ## Why Use a Stack?

    A rectangle is completely determined by two quantities:

    ```text
    Area = Height × Width
    ```

    For every bar, we would like to know:

    - the first smaller bar on its left
    - the first smaller bar on its right

    Once these two boundaries are known, the maximum rectangle using this bar as the **minimum height** is uniquely determined.

    A monotonic stack can find these boundaries in linear time.

    ## What Should Be Stored in the Stack?

    The stack stores **indices**, not heights.

    ```python
    stack = [index]
    ```

    Whenever the height is needed, simply access

    ```python
    heights[index]
    ```

    The indices are also required to compute the rectangle width.

    ## What Kind of Monotonic Stack?

    We maintain a **monotonically increasing stack**.

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
    2
    3
    5
    Bottom
    ```

    Whenever a shorter bar appears, one or more rectangles are completed.

    This is the most important idea in the entire algorithm.

    Consider the following code:

    ```python
    mid = heights[stack.pop()]
    ```

    At first glance, it is easy to think that `mid` is simply the "middle bar".

    It is **not**.

    Instead,

    > **`mid` is the height of the rectangle we are currently computing.**
    >

    The moment a bar is popped, we know that:

    - we have found the **first smaller bar on its right**
    - the stack top becomes the **first smaller bar on its left**

    Therefore, the maximum rectangle whose **minimum height is `mid`** has been completely determined.

    For example,

    ```text
    2 1 5 6 2
    ```

    When processing the last `2`, the bar with height `5` is popped.

    ```text
            █
            █
        █   █
        █   █
    █   █   █
    █ █ █ █ █
    -----------
    2 1 5 6 2
    ```

    Now we know:

    - Left smaller bar → height `1`
    - Right smaller bar → height `2`

    So the bar with height `5` can extend across:

    ```text
    5 6
    ```

    Its rectangle is

    ```text
    Height = 5
    Width  = 2
    Area   = 5 × 2 = 10
    ```

    Notice that the rectangle height is **exactly the popped bar (`mid`)**.

    This is why the area is computed as

    ```python
    area = mid * width
    ```

    rather than using either boundary height.

    **Key Idea**

    > Every time a bar is popped, we are computing the **largest rectangle whose minimum height is exactly this bar**.
    >

    One of the smartest tricks in this problem is adding two sentinel bars.

    ```python
    heights.insert(0, 0)
    heights.append(0)
    ```

    These two zeros greatly simplify the implementation.

    ## The Right Sentinel

    Consider

    ```python
    heights = [2,1,2]
    ```

    Without the trailing `0`, the traversal ends like this:

    ```text
    Stack
    -----
    1
    2
    ```

    The last two bars are still inside the stack because no shorter bar ever appeared.

    Since rectangles are only computed **when a bar is popped**, these bars would never be processed.

    Without the trailing zero, we would have to write another loop:

    ```python
    while stack:
        ...
    ```

    to empty the stack.

    Instead, we append a `0`.

    ```text
    2 1 2 0
    ```

    Now,

    ```text
    0 < 2
    ```

    so the last `2` is popped.

    Then,

    ```text
    0 < 1
    ```

    so `1` is also popped.

    The stack is automatically emptied.

    Therefore,

    > **The trailing `0` forces every remaining bar to be popped and processed.**
    >

    ## The Left Sentinel

    Now consider the same example.

    ```text
    2 1 2 0
    ```

    Eventually the bar with height `1` will also be popped.

    After popping it,

    the stack becomes empty.

    However, we still need to compute

    ```python
    width = right - left - 1
    ```

    where

    ```python
    left = stack[-1]
    ```

    If the stack is empty, there is no left boundary.

    That means we would need an extra condition such as

    ```python
    if stack:
        ...
    ```

    to avoid accessing an empty stack.

    Instead, we prepend another `0`.

    ```text
    0 2 1 2 0
    ```

    Since no height is smaller than `0`, this sentinel is never popped.

    Therefore,

    ```python
    stack[-1]
    ```

    is always valid.

    It serves as a permanent left boundary.

    ## Why Must the Sentinel Be Zero?

    The sentinel should never participate in the final answer.

    Choosing

    ```text
    0
    ```

    guarantees that:

    - every real bar is taller than it,
    - it never contributes to the maximum rectangle,
    - it naturally serves as a boundary.

    ## Summary of the Two Sentinels

    ```text
    Sentinel                     Sentinel
        │                            │
        ▼                            ▼

    0 2 1 5 6 2 3 0
    ```

    The left sentinel:

    > Guarantees that the stack never becomes empty.
    >

    The right sentinel:

    > Forces every remaining bar to be popped before the traversal ends.
    >

    Together they eliminate all boundary cases and make the algorithm much cleaner.

    During traversal, there are three situations.

    ### Case 1: Current bar is higher

    ```python
    heights[i] > heights[stack[-1]]
    ```

    Simply push the current index.

    ### Case 2: Current bar has the same height

    Some implementations replace the previous index with the current one.

    Your implementation simply keeps both indices, which also works correctly.

    ### Case 3: Current bar is lower

    ```python
    heights[i] < heights[stack[-1]]
    ```

    One or more rectangles are completed.

    Continue popping while the current bar is lower.

    ```python
    while stack and heights[i] < heights[stack[-1]]:
    ```

    For every popped bar,

    ```python
    mid = heights[stack.pop()]
    ```

    Compute

    ```python
    width = i - stack[-1] - 1
    area = mid * width
    ```

    Update the maximum area.

    Finally,

    push the current index.

    Initially, we may analyze three situations:

    - Current height is larger
    - Current height is equal
    - Current height is smaller

    However, only the third case actually performs extra work.

    The first two cases simply end with

    ```python
    stack.append(i)
    ```

    Therefore, instead of explicitly writing three branches,

    ```python
    if ...
    elif ...
    else ...
    ```

    we can directly write

    ```python
    while stack and heights[i] < heights[stack[-1]]:
        ...
    stack.append(i)
    ```

    This works because:

    - If the current height is larger, the `while` loop is skipped.
    - If the current height is equal, the `while` loop is also skipped.
    - Only when the current height is smaller do we repeatedly pop bars and compute rectangle areas.

    Thus, the three cases naturally collapse into a much cleaner implementation.

    ```python
    heights = [2,1,5,6,2,3]
    ```

    After adding sentinels:

    ```text
    0 2 1 5 6 2 3 0
    ```

    When processing the last `2`,

    the bar with height `6` is popped.

    ```text
    Height = 6
    Width  = 1
    Area   = 6
    ```

    Next,

    the bar with height `5` is popped.

    ```text
    Height = 5
    Width  = 2
    Area   = 10
    ```

    This is the largest rectangle.

    **Time Complexity**

    ```text
    O(n)
    ```

    Each index is pushed once and popped once.

    **Space Complexity**

    ```text
    O(n)
    ```

    The stack stores indices.

    **Data Structure**

    ```text
    Monotonic Increasing Stack
    ```

    **Stack Content**

    ```text
    Indices of bars
    ```

    **Rectangle Formula**

    ```python
    Height = popped bar (mid)
    Width = right_smaller - left_smaller - 1
    Area = Height × Width
    ```

    **Traversal Order**

    ```text
    Left to right
    ```

    **Final Answer**

    ```text
    The maximum rectangle area among all popped bars.
    ```

    ## Common Mistakes

    - Do **not** think that `mid` represents the "middle bar." It represents the **height of the rectangle currently being computed**.
    - Every popped bar is treated as the **minimum height** of one maximal rectangle.
    - The rectangle width is

    ```python
    i - stack[-1] - 1
    ```

    because both boundary bars are excluded.

    - Do **not** forget the two sentinel zeros.
        - The leading `0` guarantees that the stack never becomes empty.
        - The trailing `0` guarantees that every remaining bar is eventually popped.
    - Without the sentinel bars, extra boundary handling (such as clearing the stack after traversal or checking for an empty stack) would be required.
    - Although the algorithm is based on a monotonic stack, the rectangle is **not** computed when a bar is pushed. It is computed **only when the bar is popped**, because only then are both boundaries known.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 15, 2026 |

---

[Back to Day 42 Monotonic Stack II](index.md) · [All Algorithm notes](../index.md)
