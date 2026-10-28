---
hide:
  - toc
---

# 102. Binary Tree Level Order Traversal

*Edited by Charles*

**Medium · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/binary-tree-level-order-traversal/)

!!! tip "Core idea"

    Solution: length

    To retrieve nodes in the binary tree, we need to use deque. Fill the root node in the deque.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        result = []

        def traverse(node, i):
            if not node:
                return

            if len(result) == i:
                result.append([])

            result[i].append(node.val)
            traverse(node.left, i + 1)
            traverse(node.right, i + 1)

        traverse(root, 0)

        return result
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Solution: length

    1. To retrieve nodes in the binary tree, we need to use deque.
    2. Fill the root node in the deque.
    3. Iterate the queue as long as it is not empty.
        1. count the number of elements in the queue.
        2. take them out in order one by one (for loop), add each node‘s value to the layer/level list to record the values of nodes in each layer/level.
        3. spread to next layers/levels by adding the node’s left and right nodes to the queue.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    from collections import deque

    class Solution:
        def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
            if not root:
                return []

            queue = deque([root])
            result = []

            while queue:
                layer = []
                n = len(queue)

                for _ in range(n):
                    node = queue.popleft()
                    layer.append(node.val)

                    if node.left:
                        queue.append(node.left)
                    if node.right:
                        queue.append(node.right)

                result.append(layer)

            return result
    ```

    Solution: recursive

    1. Similar to BFS.
    2. The list result[i] represents the list of nodes’ values in the corresponding level.
        1. if the node is the first one in the level, i.e. i = 1, result = [ [root] ], the first level (level 0) has been complete, and now we are counting values in level 1. We see that len(result) == 1, and hence append an empty list to result for the next level.
        2. append node’s value to the empty list that was just created.
        3. recursively apply the function to node’s left and node’s right until the input node is None/NULL.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 12, 2026 |
    | Feedback | Strongly Recommended for Multiple Attempts! |

---

[Back to Day 11 Binary Tree I](index.md) · [All Algorithm notes](../index.md)
