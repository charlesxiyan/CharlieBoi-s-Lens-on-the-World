---
hide:
  - toc
---

# 222. Count Complete Tree Nodes

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/count-complete-tree-nodes/)

!!! tip "Core idea"

    Iteration:

    Reference to [144. Binary Tree Preorder Traversal](../day-11/144-binary-tree-preorder-traversal.md)

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        left, right = root, root
        leftHeight, rightHeight = 0, 0

        while left:
            left = left.left
            leftHeight += 1

        while right:
            right = right.right
            rightHeight += 1

        if leftHeight == rightHeight:
            return pow(2, leftHeight) - 1

        return 1 + self.countNodes(root.left) + self.countNodes(root.right)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Iteration:

    Reference to [144. Binary Tree Preorder Traversal](../day-11/144-binary-tree-preorder-traversal.md)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    from collections import deque

    class Solution:
        def countNodes(self, root: Optional[TreeNode]) -> int:
            if not root:
                return 0

            dq = deque([root])
            result = 0

            while dq:
                n = len(dq)

                for _ in range(n):
                    node = dq.popleft()
                    result += 1

                    if node.left:
                        dq.append(node.left)
                    if node.right:
                        dq.append(node.right)

            return result
    ```

    Recursion

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def countNodes(self, root: Optional[TreeNode]) -> int:
            return self.traverse(root)

        def traverse(self, node):
            if not node:
                return 0

            leftSum = traverse(node.left)
            rightSum = traverse(node.right)

            return leftSum + rightSum + 1
    ```

    Recursion with O(logN * logN)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |

---

[Back to Day 13 Binary Tree III](index.md) · [All LeetCode notes](../index.md)
