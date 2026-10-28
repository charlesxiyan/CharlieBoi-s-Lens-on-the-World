---
hide:
  - toc
---

# 106. Construct Binary Tree from Inorder and Postorder Traversal

*Edited by Charles*

**Medium · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

!!! tip "Core idea"

    Recursion (take inorder = [9, 3, 15, 20, 7] and postorder = [9, 15, 7, 20, 3] as an example):

    Keep in mind node order in these two orders are: left - middle (parent) - right, and left - right - middle. For postorder and preorder, we can always know the root node, while in inorder, as long as we know the middle, we know the sub binary trees of left and right.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not postorder:
            return None

        root = TreeNode(val = postorder[-1])
        idx = inorder.index(root.val)


        inorderLeft, inorderRight = inorder[:idx], inorder[idx + 1:]
        postorderLeft, postorderRight = postorder[:idx], postorder[idx:-1]

        root.left = self.buildTree(inorderLeft, postorderLeft)
        root.right = self.buildTree(inorderRight, postorderRight)

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion (take inorder = [9, 3, 15, 20, 7] and postorder = [9, 15, 7, 20, 3] as an example):

    1. Keep in mind node order in these two orders are: left - middle (parent) - right, and left - right - middle.
    2. For postorder and preorder, we can always know the root node, while in inorder, as long as we know the middle, we know the sub binary trees of left and right.
    3. The last element of postorder is always the root node.
    4. Based on the root node, partition the inorder by searching the node.val in the list and retrieving the list index - idx.
    5. Partition inorder and postorder lists:
        1. Inorder: left = [9], root = 3, right = [15, 20, 7].
        2. Postorder: left = [9], right = [15, 7, 20], root = 3.
    6. Link root.left and root.right by creating two nodes recursively.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 15, 2026 |

---

[Back to Day 14 Binary Tree IV](index.md) · [All LeetCode notes](../index.md)
