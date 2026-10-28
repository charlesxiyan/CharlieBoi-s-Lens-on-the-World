---
hide:
  - toc
---

# 617. Merge Two Binary Trees

*Edited by Charles*

**Easy · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/merge-two-binary-trees/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recursion:

    if root1 and root2 are both None, then the new tree does not have the child at this position. if either root1 or root2 exists while the other is None, make the existing node as the new node.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1 and not root2:
            return None
        if root1 and not root2:
            return root1
        if not root1 and root2:
            return root2

        root = TreeNode(root1.val + root2.val)
        root.left = self.mergeTrees(root1.left, root2.left)
        root.right = self.mergeTrees(root1.right, root2.right)

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    1. if root1 and root2 are both None, then the new tree does not have the child at this position.
    2. if either root1 or root2 exists while the other is None, make the existing node as the new node.
    3. if both two are valid, merge them by adding the values up and make a new node.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 15, 2026 |

---

[Back to Day 15 Binary Tree V](index.md) · [All Algorithm notes](../index.md)
