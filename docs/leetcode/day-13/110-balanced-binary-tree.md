---
hide:
  - toc
---

# 110. Balanced Binary Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/balanced-binary-tree/)

!!! tip "Core idea"

    Recursion

    Reference to [104. Maximum Depth of Binary Tree](../day-12/104-maximum-depth-of-binary-tree.md)

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
        self.checkBalanced = True

    def isBalanced(self, root: Optional[TreeNode])->bool:
        self.maxDepth(root)
        return self.checkBalanced

    def maxDepth(self, node):
        if not node:
            return 0

        leftHeight= self.maxDepth(node.left)
        rightHeight= self.maxDepth(node.right)

        if abs(leftHeight- rightHeight)>1:
            self.checkBalanced = False

        return 1 + max(leftHeight, rightHeight)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion

    Reference to [104. Maximum Depth of Binary Tree](../day-12/104-maximum-depth-of-binary-tree.md)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |

---

[Back to Day 13 Binary Tree III](index.md) · [All LeetCode notes](../index.md)
