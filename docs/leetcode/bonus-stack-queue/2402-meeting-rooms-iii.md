---
hide:
  - toc
---

# 2402. Meeting Rooms III

*Edited by Charles*

**Hard · Python · Two heaps · Scheduling**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/meeting-rooms-iii/description/).

    Assign meetings to numbered rooms. Use the smallest available room number; if all rooms are busy, delay the meeting without changing its duration. Earlier original starts have priority. Return the most-used room, breaking ties by smaller room number. Meetings use half-open intervals and have distinct original start times.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Two different priorities require two heaps: `availableRooms` orders free rooms by ID, while `busyPeriods` orders `(endTime, roomId)` by finishing time and then ID. Sorting meetings by their original start gives delayed meetings the required priority.

    ## Algorithm Steps

    1. Sort meetings by start time and initialize all rooms as available.
    2. Before each meeting, release every room whose scheduled end is at most its original start.
    3. If a room is available, use the smallest ID and keep the original end time.
    4. Otherwise pop the earliest available busy room and schedule the meeting for its full duration starting at that room’s end time.
    5. Count assignments and return the first index having the maximum count.

    ## Hand Simulation

    For `n = 2` and meetings `[[0,4],[1,3],[2,5],[3,4]]`:

    | Original meeting | Room | Actual interval | Reason |
    | --- | --- | --- | --- |
    | `[0,4)` | 0 | `[0,4)` | Smallest free ID |
    | `[1,3)` | 1 | `[1,3)` | Only free room |
    | `[2,5)` | 1 | `[3,6)` | Earliest release; retain duration 3 |
    | `[3,4)` | 0 | `[4,5)` | Room 1 is already reserved until 6 |

    Both rooms host two meetings, so return `0`.

    ## Important Implementation Details

    Use `<= start` when releasing rooms: a meeting ending exactly at the next start does not overlap it. Release **all** eligible rooms before choosing an ID. Tuple ordering handles equal end times automatically. A delayed end is `endTime + (end - start)`, not the original `end`. Processing sorted starts reserves rooms for earlier requests before later requests. `count.index(max(count))` implements the final tie-break. Sorting mutates `meetings`.

    ## Time and Space Complexity

    For $M$ meetings and $N$ rooms, time is $O(M\log(M+1)+M\log(N+1)+N)$. Each meeting is pushed and popped at most once in the busy heap. The heaps and counts use $O(N)$ space; Python’s in-place sort can additionally use $O(M)$ temporary space.

    ## Key Takeaways

    Keep room-ID priority separate from release-time priority, and preserve duration when delaying a meeting.

## Python solution

```python
import heapq

class Solution:
    def mostBooked(self, n: int, meetings: list[list[int]]) -> int:
        meetings.sort(key = lambda x: x[0])

        availableRooms = list(range(n))
        heapq.heapify(availableRooms)

        busyPeriods = []
        count = [0] * n

        for start, end in meetings:
            while busyPeriods and busyPeriods[0][0] <= start:
                endTime, roomId = heapq.heappop(busyPeriods)
                heapq.heappush(availableRooms, roomId)

            if availableRooms:
                roomId = heapq.heappop(availableRooms)
                heapq.heappush(busyPeriods, (end, roomId))
            else:
                endTime, roomId = heapq.heappop(busyPeriods)
                newEnd = endTime + (end - start)
                heapq.heappush(busyPeriods, (newEnd, roomId))

            count[roomId] += 1

        return count.index(max(count))
```

---

[Back to Bonus Stack & Queue](index.md) · [All Algorithm notes](../index.md)
