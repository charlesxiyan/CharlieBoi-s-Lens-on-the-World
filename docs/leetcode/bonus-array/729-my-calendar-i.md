---
hide:
  - toc
---

# 729. My Calendar I

*Edited by Charles*

**Medium · Python · Intervals · Linear scan**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/my-calendar-i/description/).

    Accept a new half-open booking `[startTime, endTime)` only when it does not overlap any accepted booking. Return whether it was accepted.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Two half-open intervals overlap exactly when their later start is strictly less than their earlier end. Scan existing bookings and reject immediately when that intersection has positive length.

    ## Algorithm Steps

    1. For each accepted `[start, end)`, compare `max(start, startTime)` with `min(end, endTime)`.
    2. If the maximum start is smaller, return `False`.
    3. Only after all comparisons pass, append the new booking and return `True`.

    ## Hand Simulation

    Book `[2,6)` → `True`. Try `[5,8)` → `False`, because `max(2,5) = 5 < min(6,8) = 6`. Book `[6,9)` → `True`, because the intersection with `[2,6)` has equal bounds at 6 and is empty.

    ## Important Implementation Details

    The strict `<` is essential for half-open intervals: touching endpoints are allowed. The max/min formula covers partial overlap and containment in either direction. No rollback is needed because the list is modified only after validation. The list need not be sorted; every accepted booking is checked.

    ## Time and Space Complexity

    With $A$ accepted bookings, a call takes $O(A)$ time and $O(1)$ working space. Across $B$ requests, worst-case time is $O(B^2)$ and persistent storage is $O(B)$.

    ## Key Takeaways

    Check a proposed change before storing it when validation can be performed against the unchanged state.

## Python solution

```python
class MyCalendar:

    def __init__(self):
        self.calendar = []

    def book(self, startTime: int, endTime: int) -> bool:
        for start, end in self.calendar:
            if max(start, startTime) < min(end, endTime):
                return False

        self.calendar.append([startTime, endTime])
        return True


# Your MyCalendar object will be instantiated and called as such:
# obj = MyCalendar()
# param_1 = obj.book(startTime,endTime)
```

---

[Back to Bonus Array](index.md) · [All Algorithm notes](../index.md)
