---
hide:
  - toc
---

# 669. Trim a Binary Search Tree

*Edited by Charles*

**Medium · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/trim-a-binary-search-tree/)

!!! tip "Core idea"

    The core idea of the solution is, even though the value of the root is less than low, the right subtree may still meet the requirement; similarly, even though the value of the root is greater than high, the left subtree may still work.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return None
        if root.val > high:
            return self.trimBST(root.left, low, high)
        if root.val < low:
            return self.trimBST(root.right, low, high)

        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    The core idea of the solution is, even though the value of the root is less than low, the right subtree may still meet the requirement; similarly, even though the value of the root is greater than high, the left subtree may still work.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 21, 2026 |

---

[Back to Day 18 Binary Tree VIII](index.md) · [All Algorithm notes](../index.md)
