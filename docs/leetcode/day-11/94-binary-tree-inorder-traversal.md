---
hide:
  - toc
---

# 94. Binary Tree Inorder Traversal

*Edited by Charles*

**Easy · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/binary-tree-inorder-traversal/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Reference to [144. Binary Tree Preorder Traversal](144-binary-tree-preorder-traversal.md) (Preorder, Postorder and Inorder are all listed!)

    Original study notes follow, including intermediate snippets and earlier attempts.

    Reference to [144. Binary Tree Preorder Traversal](144-binary-tree-preorder-traversal.md) (Preorder, Postorder and Inorder are all listed!)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 12, 2026 |

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []

        stack = []
        result = []
        cur = root

        while cur or stack:
            if cur:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                result.append(cur.val)
                cur = cur.right

        return result
```

---

[Back to Day 11 Binary Tree I](index.md) · [All Algorithm notes](../index.md)
