---
hide:
  - toc
---

# 210. Course Schedule II (Topological Sort)

*Edited by Charles*

**Medium · Python · Graph**

[Original problem](https://leetcode.com/problems/course-schedule-ii/description/)

!!! tip "Core idea"

    Use **Kahn's Algorithm (BFS Topological Sort)**.

    Unlike LeetCode 207, which only checks whether all courses can be completed, this problem requires us to **return a valid course order**.

## Python solution

```python
from collections import deque

class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = [[] for _ in range(numCourses)]
        indegree = [0] * numCourses

        for nextCourse, preCourse in prerequisites:
            graph[preCourse].append(nextCourse)
            indegree[nextCourse] += 1

        queue = deque()
        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)

        order = []

        while queue:
            course = queue.popleft()
            order.append(course)

            for nextCourse in graph[course]:
                indegree[nextCourse] -= 1

                if indegree[nextCourse] == 0:
                    queue.append(nextCourse)

        return order if len(order) == numCourses else []
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Kahn's Algorithm (BFS Topological Sort)**.

    Unlike LeetCode 207, which only checks whether all courses can be completed, this problem requires us to **return a valid course order**.

    For:

    ```python
    [nextCourse, preCourse]
    ```

    the directed edge is:

    ```text
    preCourse → nextCourse
    ```

    because `preCourse` must be completed before `nextCourse`.

    ## Graph & Indegree

    Build:

    ```python
    graph[preCourse].append(nextCourse)
    indegree[nextCourse] += 1
    ```

    - `graph[i]` = courses that can be unlocked after completing course `i`.
    - `indegree[i]` = number of prerequisites course `i` still depends on.

    Example:

    ```text
    [1, 0] → 0 → 1
    ```

    So:

    ```text
    graph[0] = [1]
    indegree[1] += 1
    ```

    ## Initialize the Queue

    Add every course with:

    ```python
    indegree[i] == 0
    ```

    to the queue.

    These courses have no remaining prerequisites and can be taken immediately.

    ## BFS / Topological Sort

    Repeatedly take a course from the queue:

    ```python
    course = queue.popleft()
    order.append(course)
    ```

    Since its indegree is `0`, all its prerequisites have already been completed.

    Then remove its outgoing edges:

    ```python
    for nextCourse in graph[course]:
        indegree[nextCourse] -= 1
    ```

    If:

    ```python
    indegree[nextCourse] == 0
    ```

    all prerequisites of `nextCourse` are now completed, so add it to the queue.

    The order in which courses are removed from the queue forms a valid **topological order**.

    ## Cycle Detection

    If there is no cycle, every course will eventually be added to `order`:

    ```python
    len(order) == numCourses
    ```

    If there is a cycle, the nodes inside the cycle can never reach `indegree == 0`, so some courses will never be processed.

    Therefore:

    ```python
    return order if len(order) == numCourses else []
    ```

    ## Key Difference from LeetCode 207

    **207 - Course Schedule**

    Only check whether all nodes can be processed:

    ```text
    count == numCourses
    ```

    **210 - Course Schedule II**

    Save the processing order:

    ```text
    order.append(course)
    ```

    and return:

    ```text
    order if len(order) == numCourses else []
    ```

    > **207:** Can we finish all courses?
    >
    >
    > **210:** In what order can we finish all courses?
    >

    ## Complexity

    - Time: **O(V + E)**
    - Space: **O(V + E)**

    where:

    - `V` = number of courses
    - `E` = number of prerequisites

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 27, 2026 |

---

[Back to Day 50 Graph VIII](index.md) · [All LeetCode notes](../index.md)
