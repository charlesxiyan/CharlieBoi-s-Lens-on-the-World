---
hide:
  - toc
---

# 347. Top K Frequent Elements

*Edited by Charles*

**Medium · Python · Stack**

[Original problem](https://leetcode.com/problems/top-k-frequent-elements/)

!!! tip "Core idea"

    Apply priority queue. In Python, it is package heapq. heapq.heappush push a tuple to the priority queue in min-heap order.

## Python solution

```python
import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        dict = {}
        n = len(nums)
        result = [0] * k

        for i in range(n):
            dict[nums[i]] = dict.get(nums[i], 0) + 1

        priority_queue = []

        for key, freq in dict.items():
            heapq.heappush(priority_queue, (freq, key))
            if len(priority_queue) > k:
                heapq.heappop(priority_queue)

        for i in range(k - 1, -1, -1):
            result[i] = heapq.heappop(priority_queue)[1]

        return result
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Apply priority queue. In Python, it is package heapq.
    2. heapq.heappush push a tuple to the priority queue in min-heap order.
    3. Once the length of the priority queue is greater than k, heapq.heappop to pop the top node of the heap from the queue.
    4. The format saved in the queue is (frequency, key), and thus we retrieve keys from queues in the reverse order, since the top node is the least frequent element.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 9, 2026 |

---

[Back to Day 10 Stack & Queue II](index.md) · [All Algorithm notes](../index.md)
