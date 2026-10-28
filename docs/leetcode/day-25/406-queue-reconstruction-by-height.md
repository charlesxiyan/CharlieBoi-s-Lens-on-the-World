---
hide:
  - toc
---

# 406. Queue Reconstruction by Height

*Edited by Charles*

**Medium · Python · Greedy**

[Original problem](https://leetcode.com/problems/queue-reconstruction-by-height/description/)

!!! tip "Core idea"

    Similar to [135. Candy](135-candy.md). We need to determine the result by one dimension and then the other. Take `people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]` as an example.

## Python solution

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        # 如此排序的原因：我们希望在处理某个人的时候，已经处理过所有比TA高的人，这样就能准确插队
        # 比如在处理[6, 1]的时候，我们已经处理了[7, 0], [7, 1]
        # 那么我们就只用关心[6, 1]前面需要有一个元素即可，也就是[7, 0]，非常自然的插入
        people.sort(key = lambda x: (-x[0], x[1]))
        queue = []

        for person in people:
            queue.insert(person[1], person)

        return queue
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Similar to [135. Candy](135-candy.md). We need to determine the result by one dimension and then the other.
    2. Take `people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]` as an example.
        1. Sort the list by the height first in the descending order. For the same heights, order them by the number of people at front in the ascending order. Thus, the sorted array is `[[7,0],[7,1],[6,1],[5,0],[5,2],[4,4]]` .
        2. Create a queue, and insert the [height, number] to the queue at the position of the number. Thus, the sorted array is `[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]` . The order in step 2.a ensures the insertion meet the requirement of number of people at the front.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 29, 2026 |
    | Feedback | Recommended for practice of Python built-in functions! |

---

[Back to Day 25 Greedy Algorithm III](index.md) · [All Algorithm notes](../index.md)
