---
hide:
  - toc
---

# 732. My Calendar III

*Edited by Charles*

**Hard · Python · Hash table · Sweep line · Maximum overlap**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/my-calendar-iii/description/).

    Accept every half-open booking `[startTime, endTime)` and return the largest number of events that overlap at any time after that booking.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Use endpoint differences to reconstruct active counts. Unlike Calendar II, no booking is rejected; keep the maximum prefix sum as the overlap answer.

    ## Algorithm Steps

    1. Add the new booking’s start and end changes to `self.record`.
    2. Reset the scan’s active count to zero.
    3. Scan all endpoints chronologically, updating `self.k` with each active count.
    4. Return `self.k`.

    ## Hand Simulation

    Bookings `[1,5)`, `[3,7)`, `[4,6)`, `[7,9)` return `1, 2, 3, 3`. The third booking creates triple overlap on `[4,5)`. The fourth touches the end of `[3,7)` but does not increase the existing maximum.

    ## Important Implementation Details

    `curr` must restart at zero for each full scan. `self.k` need not reset: bookings are only added, so the true historical maximum cannot decrease. This would no longer be sufficient if cancellation were supported. Sorting is repeated on every call in this straightforward implementation; it is not a balanced-tree or segment-tree update.

    ## Time and Space Complexity

    With $U$ stored endpoints, each call takes $O(U\log(U+1))$ time and $O(U)$ temporary space for sorting. Over $B$ calls, worst-case time is $O(B^2\log(B+1))$ and persistent storage is $O(B)$.

    ## Key Takeaways

    The same difference map supports either enforcing an overlap limit or reporting the maximum overlap.

## Python solution

```python
from collections import defaultdict

class MyCalendarThree:

    def __init__(self):
        self.record = defaultdict(int)
        self.k = 0

    def book(self, startTime: int, endTime: int) -> int:
        self.record[startTime] += 1
        self.record[endTime] -= 1

        curr = 0

        for key in sorted(self.record):
            curr += self.record[key]
            self.k = max(self.k, curr)

        return self.k


# Your MyCalendarThree object will be instantiated and called as such:
# obj = MyCalendarThree()
# param_1 = obj.book(startTime,endTime)
```

---

[Back to Bonus Hash Table](index.md) · [All Algorithm notes](../index.md)
