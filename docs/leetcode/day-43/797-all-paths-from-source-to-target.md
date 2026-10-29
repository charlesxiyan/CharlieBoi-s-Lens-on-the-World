---
hide:
  - toc
---

# 797. All Paths from Source to Target

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/all-paths-from-source-to-target/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given a Directed Acyclic Graph (DAG), return **all possible paths** from node `0` (the source) to node `n - 1` (the target).

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Given a Directed Acyclic Graph (DAG), return **all possible paths** from node `0` (the source) to node `n - 1` (the target).

    For example:

    ```python
    graph = [[1,2],[3],[3],[]]
    ```

    The graph is:

    ```text
    0
    ├──►1
    │    │
    │    ▼
    │    3
    │
    └──►2
         │
         ▼
         3
    ```

    The output is:

    ```python
    [[0,1,3],[0,2,3]]
    ```

    Instead of using recursion (DFS), this solution uses **Breadth-First Search (BFS)**.

    The key idea is:

    > Instead of storing nodes in the queue, we store **entire paths**.
    >

    Every time a path reaches the target node, it is added to the answer.

    Unlike the standard BFS that stores only vertices,

    this solution stores **complete paths**.

    ```python
    paths = [[0]]
    ```

    Initially, there is only one path:

    ```text
    0
    ```

    Each iteration expands every current path by appending one neighboring node.

    Eventually, every possible path from source to target will be generated.

    Maintain a list called

    ```python
    paths
    ```

    where each element is a complete path.

    For every path:

    - If the last node is the target,
    add the path to the answer.
    - Otherwise,
    extend the path using every outgoing edge.

    For every neighbor,

    ```python
    path + [neighbor]
    ```

    creates a **new path**.

    These newly generated paths form the next BFS level.

    Consider

    ```python
    graph = [[1,2],[3],[3],[]]
    ```

    The graph is

    ```text
    0
    ├──►1
    │    │
    │    ▼
    │    3
    │
    └──►2
         │
         ▼
         3
    ```

    Initially,

    ```text
    paths = [[0]]
    result = []
    ```

    ## Round 1

    Current paths:

    ```text
    [0]
    ```

    The last node is `0`.

    Its neighbors are

    ```text
    1
    2
    ```

    Generate two new paths:

    ```text
    [0,1]
    [0,2]
    ```

    Now

    ```text
    paths = [[0,1],[0,2]]
    ```

    ## Round 2

    Current paths:

    ```text
    [0,1]
    [0,2]
    ```

    ### Expand `[0,1]`

    The last node is `1`.

    Neighbor:

    ```text
    3
    ```

    Generate

    ```text
    [0,1,3]
    ```

    ### Expand `[0,2]`

    The last node is `2`.

    Neighbor:

    ```text
    3
    ```

    Generate

    ```text
    [0,2,3]
    ```

    Now

    ```text
    paths = [[0,1,3],[0,2,3]]
    ```

    ## Round 3

    Current paths:

    ```text
    [0,1,3]
    [0,2,3]
    ```

    Both paths end at the target node.

    Add them to the result.

    ```text
    result =
    [
        [0,1,3],
        [0,2,3]
    ]
    ```

    No new paths are generated.

    The BFS terminates.

    Notice this line:

    ```python
    path + [vertex]
    ```

    Instead of modifying the existing path,

    a **new list** is created.

    For example,

    Current path:

    ```text
    [0,1]
    ```

    Neighbor:

    ```text
    3
    ```

    New path:

    ```text
    [0,1,3]
    ```

    This prevents different paths from interfering with one another.

    If we modified the original list directly, multiple BFS branches would share the same path, producing incorrect results.

    Let

    - `V` = number of vertices
    - `E` = number of edges
    - `P` = number of valid paths

    Since every valid path must be generated,

    **Time Complexity**

    ```text
    O(P × L)
    ```

    where `L` is the average path length.

    In the worst case, the number of paths can be exponential.

    **Space Complexity**

    ```text
    O(P × L)
    ```

    because the queue stores complete paths.

    **Algorithm**

    ```text
    Breadth-First Search (BFS)
    ```

    **Queue Content**

    ```text
    Complete paths
    ```

    **Initialization**

    ```python
    paths = [[0]]
    ```

    **Expansion**

    ```python
    for neighbor in graph[path[-1]]:
        new_path = path + [neighbor]
    ```

    **Termination**

    Whenever

    ```python
    path[-1] == n - 1
    ```

    add the path to the answer.

    ## Common Mistakes

    - Do **not** store only vertices in the queue. This problem requires reconstructing every path, so the queue should store **complete paths**.
    - Do **not** modify the current path in place. Always create a new path using

    ```python
    path + [neighbor]
    ```

    to avoid affecting other BFS branches.

    - A path should be added to the answer **only** when its last node is the target.
    - Remember that this problem is defined on a **Directed Acyclic Graph (DAG)**, so there are no cycles. No visited array is needed.
    - The number of paths can be exponential, so the complexity depends on the number of generated paths rather than just the number of vertices or edges.

    ## Codes

    ```python
    class Solution:
        def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
            res = []
            l = len(graph)
            paths = [[0]]

            while paths:
                temp = []
                for path in paths:
                    if path[-1] == l - 1:
                        res.append(path)
                        continue
                    for vertex in graph[path[-1]]:
                        temp.append(path + [vertex])
                paths = temp

            return res

            # 0 - 1
            # 0 - 2
            # 1 - 3
            # 2 - 3
            # 3

            # l = 4
            # paths = [[0]]

            # paths not None
            #     temp = []
            #     path in paths:
            #         path = [0]
            #         path[-1] != 3
            #         vertex in graph[path[-1]] (graph[0]: [1, 2])
            #             vertex = 1
            #                 temp = [[0, 1]]
            #             vertex = 2
            #                 temp = [[0, 1], [0, 2]]
            #     paths = temp = [[0, 1], [0, 2]]

            # paths not None
            #     temp = []
            #     path in paths:
            #         path = [0, 1]
            #         path[-1] != 3
            #         vertex in graph[path[-1]] (graph[1]: [3])
            #             vetex = 3
            #             temp = [[0, 1, 3]]

            #         path = [0, 2]
            #         path[-1] != 3
            #         vertex in graph[path[-1]] (graph[1]: [3])
            #             vertex = 3
            #             temp = [[0, 1, 3], [0, 2, 3]]
            #     paths = temp = [[0, 1, 3], [0, 2, 3]]

            # paths not None
            #     temp = []
            #     path in paths:
            #         path = [0, 1, 3]
            #         path[-1] == 3
            #             res = [[0, 1, 3]]
            #             continue
            #         path = [0, 2, 3]
            #         path[-1] == 3
            #             res = [[0, 1, 3], [0, 2, 3]]
            #             continue
            #     paths = temp = []

            # paths is None
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | August 16, 2026 |

## Python solution

```python
from collections import deque

class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        n = len(graph)
        queue = deque([[0]])
        res = []

        while queue:
            for _ in range(len(queue)):
                curPath = queue.popleft()

                for nextNode in graph[curPath[-1]]:
                    newPath = curPath + [nextNode]
                    if nextNode == n - 1:
                        res.append(newPath[:])
                    else:
                        queue.append(newPath[:])

        return res
```

---

[Back to Day 43 Graph I](index.md) · [All Algorithm notes](../index.md)
