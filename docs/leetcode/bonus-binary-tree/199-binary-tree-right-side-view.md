---
hide:
  - toc
---

# 199. Binary Tree Right Side View

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/binary-tree-right-side-view/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Given the `root` of a binary tree, imagine yourself standing on the **right side** of the tree.

    Return the values of the nodes that you can see, ordered from top to bottom.

## Python solution

```python
from collections import deque

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []

        res = []
        queue = deque([root])

        while queue:

            for i in range(len(queue)):
                curNode = queue.popleft()

                if i == 0:
                    res.append(curNode.val)

                if curNode.right:
                    queue.append(curNode.right)

                if curNode.left:
                    queue.append(curNode.left)

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given the `root` of a binary tree, imagine yourself standing on the **right side** of the tree.

    Return the values of the nodes that you can see, ordered from top to bottom.

    ## Example

    ```text
            1
           / \
          2   3
           \   \
            5   4
    ```

    From the right side, we can see:

    ```text
    1
    3
    4
    ```

    Therefore:

    ```text
    Output: [1, 3, 4]
    ```

    The key observation is:

    > For every level of the binary tree, we only need the **rightmost node**.
    >

    Therefore, this problem can be solved using **BFS (Breadth-First Search)** / **Level Order Traversal**.

    Normally, level order traversal processes nodes:

    ```text
    left → right
    ```

    But in this solution, we intentionally put the **right child into the queue first**:

    ```text
    right → left
    ```

    Therefore, the **first node processed at each level** is the rightmost node.

    We add that node to the result.

    If the tree is empty:

    ```python
    if not root:
        return []
    ```

    Otherwise, initialize the queue with the root:

    ```python
    queue = deque([root])
    ```

    and create the result:

    ```python
    res = []
    ```

    We use:

    ```python
    while queue:
    ```

    At the beginning of every iteration, `queue` contains all nodes from the current level.

    For example:

    ```text
            1
           / \
          2   3
    ```

    Initially:

    ```text
    queue = [1]
    ```

    After processing the first level:

    ```text
    queue = [3, 2]
    ```

    Notice that we add `3` before `2` because we process:

    ```text
    right child first
    left child second
    ```

    At the beginning of each level:

    ```python
    for i in range(len(queue)):
    ```

    `len(queue)` represents the number of nodes in the **current level**.

    For example:

    ```text
            1
           / \
          2   3
    ```

    When processing the second level:

    ```text
    queue = [3, 2]
    ```

    so:

    ```text
    len(queue) = 2
    ```

    Therefore:

    ```python
    for i in range(2):
    ```

    processes exactly:

    ```text
    3, 2
    ```

    During this loop, their children may be added to the queue, but those new nodes belong to the **next level** and will not be processed until the next `while` iteration.

    This is the standard technique for Level Order Traversal.

    Because we insert the **right child first**:

    ```python
    if curNode.right:
        queue.append(curNode.right)

    if curNode.left:
        queue.append(curNode.left)
    ```

    nodes at each level are processed from:

    ```text
    right → left
    ```

    Therefore, the first node:

    ```python
    i == 0
    ```

    is the rightmost node of that level.

    So:

    ```python
    if i == 0:
        res.append(curNode.val)
    ```

    Consider:

    ```text
            1
           / \
          2   3
           \   \
            5   4
    ```

    Initially:

    ```text
    queue = [1]
    res = []
    ```

    ## Level 1

    ```text
    queue = [1]
    ```

    Process:

    ```text
    i = 0
    curNode = 1
    ```

    Since:

    ```text
    i == 0
    ```

    add `1`:

    ```text
    res = [1]
    ```

    Add right child first, then left child:

    ```text
    queue = [3, 2]
    ```

    ## Level 2

    At the beginning:

    ```text
    queue = [3, 2]
    ```

    The current level contains 2 nodes.

    ### Process `3`

    ```text
    i = 0
    ```

    Since this is the first node:

    ```text
    res = [1, 3]
    ```

    Add its right child:

    ```text
    queue = [2, 4]
    ```

    ### Process `2`

    ```text
    i = 1
    ```

    Do not add it to `res`.

    Its right child is `5`, so:

    ```text
    queue = [4, 5]
    ```

    After finishing this level:

    ```text
    queue = [4, 5]
    ```

    ## Level 3

    ```text
    queue = [4, 5]
    ```

    Process `4` first:

    ```text
    i = 0
    ```

    Therefore:

    ```text
    res = [1, 3, 4]
    ```

    The answer is:

    ```text
    [1, 3, 4]
    ```

    The important relationship is:

    ```text
    Right child enters queue first
            ↓
    Nodes at next level are ordered right → left
            ↓
    First node of each level is the rightmost node
            ↓
    if i == 0:
        record the node
    ```

    Therefore:

    ```python
    if curNode.right:
        queue.append(curNode.right)

    if curNode.left:
        queue.append(curNode.left)
    ```

    works together with:

    ```python
    if i == 0:
        res.append(curNode.val)
    ```

    There is another common way to solve this problem.

    We can insert nodes in the normal order:

    ```text
    left → right
    ```

    Then the **last node** of every level is the rightmost node.

    That would be:

    ```python
    if i == levelSize - 1:
        res.append(curNode.val)
    ```

    So there are two equivalent strategies:

    ```text
    Strategy 1:

    right → left
    record first node
    i == 0
    ```

    or:

    ```text
    Strategy 2:

    left → right
    record last node
    i == levelSize - 1
    ```

    Your solution uses the first strategy.

    ```text
    Start with root
          ↓
    queue = [root]
          ↓
    Process one level at a time
          ↓
    Store current level size
          ↓
    Pop nodes from queue
          ↓
    First node of level?
          ↓
    Yes → add to res
          ↓
    Add right child
          ↓
    Add left child
          ↓
    Process next level
    ```

    The key pattern is:

    ```text
    Right first + First node
            ↓
    Right Side View
    ```

    ## Time Complexity

    Every node is added to and removed from the queue exactly once:

    ```text
    O(n)
    ```

    where `n` is the number of nodes.

    ## Space Complexity

    The queue may contain an entire level of the tree:

    ```text
    O(n)
    ```

    in the worst case.

    More precisely, it is:

    ```text
    O(w)
    ```

    where `w` is the maximum width of the binary tree.



    LeetCode 199 is essentially a **Level Order Traversal** problem.

    The special idea in this solution is:

    ```text
    Process right child before left child
                    ↓
    First node at every level
                    ↓
    Rightmost node
    ```

    Therefore:

    ```python
    if i == 0:
        res.append(curNode.val)
    ```

    The pattern to remember is:

    ```text
    BFS + level-by-level traversal
    +
    right first
    +
    record the first node of each level
    =
    Binary Tree Right Side View
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 10, 2026 |

---

[Back to Bonus Binary Tree](index.md) · [All Algorithm notes](../index.md)
