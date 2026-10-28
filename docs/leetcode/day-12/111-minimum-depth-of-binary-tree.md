---
hide:
  - toc
---

# 111. Minimum Depth of Binary Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

!!! tip "Core idea"

    Iteration:

    Reference to [102. Binary Tree Level Order Traversal](../day-11/102-binary-tree-level-order-traversal.md) Pay attention to the terminal condition, which is the current node has no either left node or right node. The counter for the min depth needs to be calculated before the iteration through the nodes in the same level.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        return self.traverse(root)

    def traverse(self, node):
        if not node:
            return 0

        leftHeight = self.traverse(node.left)
        rightHeight = self.traverse(node.right)

        if node.left and not node.right:
            return 1 + leftHeight
        if not node.left and node.right:
            return 1 + rightHeight

        return 1 + min(leftHeight, rightHeight)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Iteration:

    1. Reference to [102. Binary Tree Level Order Traversal](../day-11/102-binary-tree-level-order-traversal.md)
    2. Pay attention to the terminal condition, which is the current node has no either left node or right node. The counter for the min depth needs to be calculated before the iteration through the nodes in the same level.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    from collections import deque

    class Solution:
        def minDepth(self, root: Optional[TreeNode]) -> int:
            if not root:
                return 0

            dq = deque([root])
            result = 0

            while dq:
                n = len(dq)
                result += 1

                for _ in range(n):
                    node = dq.popleft()

                    if not node.left and not node.right:
                        return result

                    if node.left:
                        dq.append(node.left)
                    if node.right:
                        dq.append(node.right)
    ```

    Recursion:

    1. similar to [104. Maximum Depth of Binary Tree](104-maximum-depth-of-binary-tree.md)
    2. pay attention to extreme cases: one side child node is None. The minimum depth is defined as the depth of the non-None side.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |

---

[Back to Day 12 Binary Tree II](index.md) · [All Algorithm notes](../index.md)
