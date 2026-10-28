---
hide:
  - toc
---

# 104. Maximum Depth of Binary Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Iteration:

    Reference to [102. Binary Tree Level Order Traversal](../day-11/102-binary-tree-level-order-traversal.md)

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        return self.traverse(root)

    def traverse(self, node):
        if not node:
            return 0

        leftHeight = self.traverse(node.left)
        rightHeight = self.traverse(node.right)

        return 1 + max(leftHeight, rightHeight)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Iteration:

    1. Reference to [102. Binary Tree Level Order Traversal](../day-11/102-binary-tree-level-order-traversal.md)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    from collections import deque

    class Solution:
        def maxDepth(self, root: Optional[TreeNode]) -> int:
            if not root:
                return 0

            dq = deque([root])
            result = 0

            while dq:
                n = len(dq)

                for _ in range(n):
                    node = dq.popleft()

                    if node.left:
                        dq.append(node.left)
                    if node.right:
                        dq.append(node.right)

                result += 1

            return result
    ```

    Recursion:

    1. For every node, the max depth of the current node is the max height from the leaf to this node.
    2. In each recursion, make sure the returned value is added by 1.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |

---

[Back to Day 12 Binary Tree II](index.md) · [All Algorithm notes](../index.md)
