---
hide:
  - toc
---

# 530. Minimum Absolute Difference in BST

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

!!! tip "Core idea"

    Iteration:

    Reference to [98. Validate Binary Search Tree](../day-15/98-validate-binary-search-tree.md) for the core idea in an order traversal.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.minDiff = float('inf')
        self.pre = None

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.traverse(root)
        return self.minDiff

    def traverse(self, node):
        if not node:
            return

        self.traverse(node.left)

        if self.pre:
            self.minDiff = min(self.minDiff, node.val - self.pre.val)
        self.pre = node

        self.traverse(node.right)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Iteration:

    Reference to [98. Validate Binary Search Tree](../day-15/98-validate-binary-search-tree.md) for the core idea in an order traversal.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
            minDiff = float('inf')

            stack = []
            cur = root
            pre = None

            while cur or stack:
                if cur:
                    stack.append(cur)
                    cur = cur.left
                else:
                    cur = stack.pop()
                    if pre:
                        minDiff = min(minDiff, cur.val - pre.val)
                    pre = cur
                    cur = cur.right

            return minDiff
    ```

    Recursion:

    Still the basic idea of the inorder traversal but in the recursive function.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 16, 2026 |

---

[Back to Day 16 Binary Tree VI](index.md) · [All Algorithm notes](../index.md)
