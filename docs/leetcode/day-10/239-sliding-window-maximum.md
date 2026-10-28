---
hide:
  - toc
---

# 239. Sliding Window Maximum

*Edited by Charles*

**Hard · Python · Stack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/sliding-window-maximum/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    The main idea is to apply the monotonic queue. For a simulation, take [1, 3, -1, -3, 5, 3, 6, 7] and k = 3 as an example.

## Python solution

```python
from collections import deque

class MonotonicQueue:
    def __init__(self):
        self.maxq = deque()

    def push(self, n):
        while len(self.maxq) > 0 and self.maxq[-1] < n:
            self.maxq.pop()
        self.maxq.append(n)

    def pop(self, n):
        if n == self.maxq[0]:
            self.maxq.popleft()

    def max(self):
        return self.maxq[0]

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)

        window = MonotonicQueue()
        result = []

        for i in range(n):
            if i < k - 1:
                window.push(nums[i])
            else:
                window.push(nums[i])
                result.append(window.max())
                window.pop(nums[i - k + 1])

        return result
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. The main idea is to apply the monotonic queue.
    2. For a simulation, take [1, 3, -1, -3, 5, 3, 6, 7] and k = 3 as an example.
        1. i = 0, push 1 to window; [1] v.s. [1].
        2. i = 1, push 3 to window (popleft nums[0] = 1); we need to keep the maximum at the queue head for convenience; [1, 3] v.s. [3].
        3. i = 2, push -1 to window, append the head of the queue (max = 3) to the result list and popleft nums[0] from the queue (nums[0] = 1 was popped out already); [1, 3, -1] v.s. [3, -1].
        4. i = 3, push -3 to window, append the current max = 3 to the result list and popleft nums[1] = 3 from the queue; [3, -1, -3] v.s. [-1, -3].
        5. i = 4, push 5 to window (popleft smaller values at the front nums[2] = -1, nums[3] = -3), append the current max = 5 to the result list and popleft nums[2] = -1 from the queue; [-1, -3, 5] v.s. [5].
        6. i = 5, push 3 to window, append the current max = 5 to the result list and popleft nums[3] = -3 from the queue (already deleted); [-3, 5, 3] v.s. [5, 3].
        7. i = 6, push 6 to window (popleft smaller values at the front nums[4] = 5, nums[5] = 3), append the current max = 6 to the result list and popleft nums[4] = 5 from the queue (already deleted); [5, 3, 6] v.s. [6].
        8. i = 7, push 7 to window (popleft smaller values at the front nums[6] = 6), append the current max = 7 to the result list and popleft nums[5] = 3 from the queue (already deleted); [3, 6, 7] v.s. [7].

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 9, 2026 |

---

[Back to Day 10 Stack & Queue II](index.md) · [All Algorithm notes](../index.md)
