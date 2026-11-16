---
hide:
  - toc
---

# 295. Find Median from Data Stream

*Edited by Charles*

**Hard · Python · Heap · Two heaps · Data stream**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-median-from-data-stream/description/).

    Support inserting integers into a stream and querying the median of all inserted values. For an even count, return the average of the two middle values. Queries occur after at least one insertion.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Keep the lower half in `maxHeap` (stored as negatives) and the upper half in `minHeap`. Every lower-half value is at most every upper-half value. The lower half has either the same size as the upper half or one extra element.

    ## Algorithm Steps

    1. Push the new value into the lower half.
    2. Move its largest value to the upper half to restore ordering.
    3. If the upper half is larger, move its smallest value back.
    4. Read the lower maximum for an odd total, or average both roots for an even total.

    ## Hand Simulation

    Insert `1`: lower `[1]`, upper `[]`, median `1`. Insert `2`: lower `[1]`, upper `[2]`, median `1.5`. Insert `3`: lower contains `[1,2]`, upper `[3]`, median `2`. These are logical values, not the heaps’ internal array order.

    ## Important Implementation Details

    The first transfer moves the largest lower value, ensuring it cannot stay on the wrong side of the partition. The optional transfer moves the smallest upper value, preserving the ordering while balancing sizes. Only roots matter for the median; heaps are not fully sorted. Negation must be undone when moving values between heaps or reading the lower root.

    ## Time and Space Complexity

    After $N$ insertions, storage is $O(N)$. Each insertion takes $O(\log(N+1))$ time; each median query takes $O(1)$.

    ## Key Takeaways

    A median is a boundary between two balanced halves; two heaps maintain that boundary without sorting the whole stream.

## Python solution

```python
import heapq
class MedianFinder:
    def __init__(self):
        self.minHeap = []
        self.maxHeap = []
    def addNum(self, num: int) -> None:
        # minHeap stores the larger half of the values.
        # maxHeap stores the smaller half as negated values.
        heapq.heappush(self.maxHeap, -num)
        heapq.heappush(self.minHeap, -heapq.heappop(self.maxHeap))
        if len(self.maxHeap) < len(self.minHeap):
            heapq.heappush(self.maxHeap, -heapq.heappop(self.minHeap))
    def findMedian(self) -> float:
        if len(self.maxHeap) > len(self.minHeap):
            return -self.maxHeap[0]
        else:
            return (-self.maxHeap[0] + self.minHeap[0]) / 2
# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

---

[Back to Bonus Stack & Queue](index.md) · [All Algorithm notes](../index.md)
