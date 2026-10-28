---
hide:
  - toc
---

# 739. Daily Temperatures

*Edited by Charles*

**Medium · Python · Monotonic Stack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/daily-temperatures/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given an array `temperatures`, return an array `answer` where:

    is the number of days you have to wait after day `i` to get a warmer temperature.

## Python solution

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        l = len(temperatures)
        stack = [0]
        res = [0] * l

        for i in range(1, l):
            if temperatures[i] <= temperatures[stack[-1]]:
                stack.append(i)
            else:
                while len(stack) != 0 and temperatures[i] > temperatures[stack[-1]]:
                    idx = stack.pop()
                    res[idx] = i - idx
                stack.append(i)

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given an array `temperatures`, return an array `answer` where:

    ```text
    answer[i]
    ```

    is the number of days you have to wait after day `i` to get a warmer temperature.

    If there is no future day with a higher temperature, the answer is `0`.

    For example:

    ```python
    temperatures = [73,74,75,71,69,72,76,73]
    ```

    Output:

    ```python
    [1,1,4,2,1,1,0,0]
    ```

    The key observation is:

    > For each element, we need to find **the first greater element on its right**.
    >

    Whenever a problem asks for the **next greater/smaller element** in a one-dimensional array, a **monotonic stack** should be considered.

    Using a monotonic stack reduces the time complexity from **O(n²)** (brute force) to **O(n)**.

    ## Monotonic Stack

    ### Why Use a Stack?

    During traversal, we need to remember the previous elements whose next greater element has **not** been found yet.

    Instead of repeatedly scanning backward, we store these indices in a stack.

    Each index is pushed and popped at most once, leading to an overall time complexity of **O(n)**.

    ### What Should Be Stored in the Stack?

    The stack stores **indices**, not temperatures.

    ```python
    stack = [index]
    ```

    Whenever the temperature is needed, simply access

    ```python
    temperatures[index]
    ```

    Storing indices also makes it easy to compute the waiting days:

    ```python
    current_index - previous_index
    ```

    ### What Kind of Monotonic Stack?

    This problem requires finding the **first greater element to the right**.

    Therefore, we maintain a **monotonically decreasing stack of temperatures**.

    Equivalently,

    > The temperatures corresponding to the indices in the stack are decreasing from **bottom to top**.
    >

    For example,

    ```text
    Temperatures:
    75
    73
    71
    69
    ```

    Corresponding stack:

    ```text
    Bottom
    75
    73
    71
    69
    Top
    ```

    The stack stores indices, while the temperatures remain in decreasing order.

    ## State Transition

    During traversal, there are three situations.

    ### Case 1: Current temperature is smaller

    If

    ```python
    temperatures[i] < temperatures[stack[-1]]
    ```

    the current day cannot help any previous day.

    Simply push its index into the stack.

    ```python
    stack.append(i)
    ```

    ### Case 2: Current temperature is equal

    If

    ```python
    temperatures[i] == temperatures[stack[-1]]
    ```

    we still push it into the stack.

    This problem asks for the **first strictly greater temperature**, not greater than or equal to.

    ```python
    stack.append(i)
    ```

    ### Case 3: Current temperature is greater

    If

    ```python
    temperatures[i] > temperatures[stack[-1]]
    ```

    we have found the next warmer day for the stack top.

    Continue popping while the current temperature is greater.

    ```python
    while stack and temperatures[i] > temperatures[stack[-1]]:
        idx = stack.pop()
        result[idx] = i - idx
    ```

    After all smaller temperatures are processed,

    push the current index.

    ```python
    stack.append(i)
    ```

    ## Example

    ```python
    temperatures = [73,74,75,71,69,72,76,73]
    ```

    Initialization:

    ```text
    Stack: [0]
    Result: [0,0,0,0,0,0,0,0]
    ```

    ### Day 1 (74)

    ```text
    74 > 73
    ```

    Pop index `0`.

    Update:

    ```text
    result[0] = 1
    ```

    Push index `1`.

    ```text
    Stack = [1]
    ```

    ### Day 2 (75)

    ```text
    75 > 74
    ```

    Pop index `1`.

    ```text
    result[1] = 1
    ```

    Push index `2`.

    ```text
    Stack = [2]
    ```

    ### Day 3 (71)

    ```text
    71 < 75
    ```

    Push directly.

    ```text
    Stack = [2,3]
    ```

    ### Day 4 (69)

    ```text
    69 < 71
    ```

    Push directly.

    ```text
    Stack = [2,3,4]
    ```

    ### Day 5 (72)

    ```text
    72 > 69
    ```

    Pop:

    ```text
    result[4] = 1
    ```

    Continue:

    ```text
    72 > 71
    ```

    Pop again:

    ```text
    result[3] = 2
    ```

    Now

    ```text
    72 < 75
    ```

    Stop popping.

    Push index `5`.

    ```text
    Stack = [2,5]
    ```

    ### Day 6 (76)

    Pop until the stack becomes empty.

    ```text
    76 > 72
    result[5] = 1

    76 > 75
    result[2] = 4
    ```

    Push index `6`.

    ```text
    Stack = [6]
    ```

    ### Day 7 (73)

    ```text
    73 < 76
    ```

    Push directly.

    Final stack:

    ```text
    [6,7]
    ```

    These indices never found a warmer temperature, so their answers remain `0`.

    Final result:

    ```python
    [1,1,4,2,1,1,0,0]
    ```

    ## Complexity Analysis

    **Time Complexity**

    ```text
    O(n)
    ```

    Each index is pushed into and popped from the stack at most once.

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
    Indices of temperatures
    ```

    **Monotonic Property**

    ```text
    Temperatures decrease from bottom to top.
    ```

    **Transition**

    ```python
    while stack and temperatures[i] > temperatures[stack[-1]]:
        idx = stack.pop()
        result[idx] = i - idx

    stack.append(i)
    ```

    **Traversal Order**

    ```text
    Left to right
    ```

    **Final Answer**

    ```text
    The result array stores the waiting days for each index.
    ```

    ## Common Mistakes

    1. Store **indices**, not temperatures.

        We need indices to compute:


    ```python
    i - previous_index
    ```

    1. The comparison should be:

    ```python
    temperatures[i] > temperatures[stack[-1]]
    ```

    because the problem asks for the **next strictly warmer day**.

    1. Continue popping while the current temperature is greater.

    Do **not** stop after popping only one element.

    ```python
    while stack and temperatures[i] > temperatures[stack[-1]]:
    ```

    1. Push the current index **after** processing all smaller temperatures.
    2. Initialize the result array with zeros.

    If an index is never popped from the stack, it means there is no warmer future day, and its answer should remain 0.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 13, 2026 |

---

[Back to Day 41 Monotonic Stack I](index.md) · [All Algorithm notes](../index.md)
