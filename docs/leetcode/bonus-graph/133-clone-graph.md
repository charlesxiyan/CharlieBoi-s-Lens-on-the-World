---
hide:
  - toc
---

# 133. Clone Graph

*Edited by Charles*

**Medium · Python · Graph · BFS & HashMap**

## 1. Problem Description

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/clone-graph/description/).

    Given a node in a connected undirected graph, return the corresponding node
    of a deep-copied graph. Each node has a value and a list of neighbor nodes.

    Preserve the values and adjacency relationships, but create new node objects.
    No cloned neighbor reference should point into the original graph.
    For an empty graph, return `None`.

## 2. Approach and Core Idea

!!! tip "Clone each node once; copy every neighbor relationship"
    Use BFS to visit original nodes and a dictionary `copies` to map each original
    node object to its one cloned node object.

The dictionary solves two problems at once:

1. **Identity:** it tells us which clone corresponds to a given original node.
2. **Discovery:** a key already present means we have created that node's clone
   and do not need to enqueue the original again.

A cycle makes naive repeated copying fail: following `A → B → A` would create
new copies forever. Reusing `copies[A]` closes the cycle in the cloned graph.

**Why it works:** every discovered original node receives exactly one clone.
When processing a node, we append the corresponding clone of every neighbor.
Thus all reachable nodes and their adjacency lists are reproduced without
sharing any original nodes.

## 3. Algorithm Steps

1. If the input node is `None`, return `None`.
2. Create its clone, record the mapping, and put the original node into a `deque`.
3. Pop an original node from the queue.
4. For each original neighbor:
   - If it has no clone yet, create one, record it, and enqueue the original neighbor.
   - **Always** append the neighbor's clone to the current node's cloned neighbor list.
5. Repeat until the queue is empty, then return the clone of the input node.

## 4. Hand Simulation

Use a triangle, which contains a cycle. Letters name node identities:

```text
A(1) ----- B(2)
  \       /
    C(3)

A.neighbors = [B, C]
B.neighbors = [A, C]
C.neighbors = [A, B]
```

Let `A'`, `B'`, and `C'` denote the new nodes.

| Step | Queue after processing | New clones | Connections appended |
| --- | --- | --- | --- |
| Initialize | `[A]` | `A'` | None yet |
| Process A | `[B, C]` | `B'`, `C'` | `A' → B'`, `A' → C'` |
| Process B | `[C]` | None | `B' → A'`, `B' → C'` |
| Process C | `[]` | None | `C' → A'`, `C' → B'` |

When processing B, both A and C are already in `copies`. We do not enqueue them
again, but their edges must still be appended. The result has the same triangle
structure, using three entirely new objects.

## 5. Important Implementation Details

### A visited neighbor still needs an edge

These two operations have different conditions:

```python
if neighbor not in copies:
    copies[neighbor] = Node(neighbor.val)
    queue.append(neighbor)

copies[current].neighbors.append(copies[neighbor])
```

The `if` controls **node creation and queueing**, not edge creation. Moving the
append inside the `if` would skip B's connection back to A and B's connection to
C in the example. Discovering a node once is enough; copying only discovery edges
would lose part of the graph.

### Use original node objects as dictionary keys

The mapping describes object identity, not just values. LeetCode gives unique
values for this problem, but object keys directly express the cloning relationship
and also work when applying this pattern to graphs with repeated values.

The supplied/default `Node` class is hashable by identity. If adapting this code
to a custom class that overrides equality, preserve compatible identity hashing.

### Give each clone its own neighbor list

Do not assign `clone.neighbors = original.neighbors`: that would share the old
list and old nodes. The constructor below creates a fresh empty list per clone.
`deque.popleft()` removes the next BFS item efficiently.

## 6. Time and Space Complexity

Let $V$ be the number of reachable nodes and $E$ the number of undirected edges.
Each undirected edge appears in two neighbor lists, which only changes the constant.

| Resource | Complexity | Reason |
| --- | --- | --- |
| Time | $O(V+E)$ | Each node is processed once and every adjacency entry is copied. |
| Auxiliary space | $O(V)$ | The dictionary and queue store original/clone references. |
| Output space | $O(V+E)$ | The new graph contains all cloned nodes and adjacency entries. |

## 7. Complete Python Solution

LeetCode already supplies `Node`; its definition is included here so the example
can also be run locally.

```python
from collections import deque
from typing import Optional


class Node:
    def __init__(self, val: int = 0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []


class Solution:
    def cloneGraph(self, node: Optional[Node]) -> Optional[Node]:
        if node is None:
            return None

        copies = {node: Node(node.val)}
        queue = deque([node])

        while queue:
            current = queue.popleft()
            for neighbor in current.neighbors:
                if neighbor not in copies:
                    copies[neighbor] = Node(neighbor.val)
                    queue.append(neighbor)

                # Copy the edge even if the neighbor was already discovered.
                copies[current].neighbors.append(copies[neighbor])

        return copies[node]
```

## 8. Key Takeaways

- A deep copy preserves relationships, not object identities.
- The original-to-clone dictionary prevents duplicate clones and endless cycles.
- Create each node once, but copy every adjacency entry.
- BFS controls traversal order; the dictionary preserves the graph's structure.

---

[Back to Bonus Graph](index.md) · [All Algorithm notes](../index.md)
