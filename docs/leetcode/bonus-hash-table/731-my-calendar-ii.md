---
hide:
  - toc
---

# 731. My Calendar II

*Edited by Charles*

**Medium · Python · Hash table · Sweep line · Rollback**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/my-calendar-ii/description/).

    Accept a new half-open booking `[startTime, endTime)` only if no time would be covered by three events. Double bookings are allowed; rejected bookings must leave the accepted schedule unchanged.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Tentatively add the new interval’s endpoint changes, then scan the resulting active counts. If a count reaches three, undo both changes. This is a small transaction: apply, validate, then keep or roll back.

    ## Algorithm Steps

    1. Add `+1` at the requested start and `-1` at its end.
    2. Scan sorted endpoint keys with `curr = 0`.
    3. If `curr >= 3`, reverse both updates and return `False`.
    4. Otherwise retain the updates and return `True`.

    ## Hand Simulation

    Book `[1,5)` → `True`. Book `[3,7)` → `True`, with two active events on `[3,5)`. Try `[4,6)` → `False`: the tentative count reaches three on `[4,5)`, so undo its `+1` at 4 and `-1` at 6. Book `[5,8)` → `True`: the first event ends at 5, leaving at most two active events.

    ## Important Implementation Details

    Rollback must undo **both** endpoints. Otherwise later requests would be checked against a corrupted schedule. Returning early is safe because the scan only reads prefix sums; there are no other mutations to reverse. Zero-valued dictionary entries may remain after rollback; they do not affect correctness, but still contribute to sorting cost. Equal-time changes are combined before checking, so touching endpoints do not create a triple booking.

    ## Time and Space Complexity

    After $B$ requests, there are at most $2B$ stored keys, including keys from rejected requests. A request takes $O(U\log(U+1))$ time for $U$ keys and $O(U)$ temporary sorting space. Total worst-case time over $B$ requests is $O(B^2\log(B+1))$, with $O(B)$ persistent storage.

    ## Key Takeaways

    A tentative update is safe only when every mutation can be reversed on rejection.

## Python solution

```python
from collections import defaultdict

class MyCalendarTwo:

    def __init__(self):
        self.record = defaultdict(int)

    def book(self, startTime: int, endTime: int) -> bool:
        self.record[startTime] += 1
        self.record[endTime] -= 1

        curr = 0

        for key in sorted(self.record):
            curr += self.record[key]

            if curr >= 3:
                self.record[startTime] -= 1
                self.record[endTime] += 1
                return False

        return True


# Your MyCalendarTwo object will be instantiated and called as such:
# obj = MyCalendarTwo()
# param_1 = obj.book(startTime,endTime)
```

---

[Back to Bonus Hash Table](index.md) · [All Algorithm notes](../index.md)
