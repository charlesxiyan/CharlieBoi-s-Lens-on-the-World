---
hide:
  - toc
---

# 105. Construct Binary Tree from Preorder and Inorder Traversal

*Edited by Charles*

**Medium · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

!!! tip "Core idea"

    Reference to [106. Construct Binary Tree from Inorder and Postorder Traversal](106-construct-binary-tree-from-inorder-and-postorder-traversal.md)  for the core idea.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder:
            return None

        val = preorder[0]
        root = TreeNode(val)

        idx = inorder.index(val)

        inorderLeft, inorderRight = inorder[: idx], inorder[idx + 1:]
        preorderLeft, preorderRight = preorder[1: idx + 1], preorder[idx + 1:]

        root.left = self.buildTree(preorderLeft, inorderLeft)
        root.right = self.buildTree(preorderRight, inorderRight)

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Reference to [106. Construct Binary Tree from Inorder and Postorder Traversal](106-construct-binary-tree-from-inorder-and-postorder-traversal.md)  for the core idea.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 15, 2026 |

---

[Back to Day 14 Binary Tree IV](index.md) · [All Algorithm notes](../index.md)
