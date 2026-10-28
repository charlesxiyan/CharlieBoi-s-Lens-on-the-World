---
hide:
  - toc
---

# 207. Course Schedule (Topological Sort)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/course-schedule/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Use **Kahn's Algorithm (BFS Topological Sort)** to detect whether the directed graph contains a cycle.

## Python solution

```python
from collections import deque

class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = [[] for _ in range(numCourses)]
        indegree = [0] * numCourses

        for nextCourse, preCourse in prerequisites:
            graph[preCourse].append(nextCourse)
            indegree[nextCourse] += 1

        queue = deque()
        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)

        count = 0

        while queue:
            course = queue.popleft()
            count += 1

            for nextCourse in graph[course]:
                indegree[nextCourse] -= 1

                if indegree[nextCourse] == 0:
                    queue.append(nextCourse)

        return count == numCourses
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Kahn's Algorithm (BFS Topological Sort)** to detect whether the directed graph contains a cycle.

    For:

    ```python
    [nextCourse, preCourse]
    ```

    `preCourse` must be completed before `nextCourse`, so the directed edge is:

    ```text
    preCourse → nextCourse
    ```

    If we can process all courses using topological sorting, there is no cycle.

    ## Graph & Indegree

    ```python
    graph[preCourse].append(nextCourse)
    indegree[nextCourse] += 1
    ```

    - `graph[i]` = all courses that can be unlocked after completing course `i`.
    - `indegree[i]` = number of prerequisites that course `i` still depends on.

    Example:

    ```text
    [1, 0] → 0 → 1
    ```

    Therefore:

    ```text
    graph[0] = [1]
    indegree[1] += 1
    ```

    ## Initialize the Queue

    Add all courses with:

    ```python
    indegree[i] == 0
    ```

    to the queue.

    These courses have **no prerequisites**, so they can be taken immediately.

    ## BFS / Kahn's Algorithm

    Repeatedly remove a course with `indegree == 0` from the queue.

    After completing `course`:

    ```python
    for nextCourse in graph[course]:
        indegree[nextCourse] -= 1
    ```

    This means one prerequisite of `nextCourse` has been completed.

    If:

    ```python
    indegree[nextCourse] == 0
    ```

    then all prerequisites of `nextCourse` are satisfied, so it can be added to the queue.

    ## Why Does This Detect a Cycle?

    Keep a `count` of how many courses have been processed.

    If there is **no cycle**, eventually every course can reach `indegree == 0` and be processed:

    ```text
    count == numCourses
    ```

    If there is a **cycle**, the courses inside the cycle will always depend on each other, so their indegrees can never become `0`.

    Therefore:

    ```text
    count < numCourses
    ```

    means a cycle exists.

    ### Key Idea

    > Continuously remove nodes with indegree `0`.
    >
    >
    > If all nodes can be removed, the directed graph has no cycle.
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

[Back to Day 50 Graph VIII](index.md) · [All Algorithm notes](../index.md)
