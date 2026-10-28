---
hide:
  - toc
---

# 700. Search in a Binary Search Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)

!!! tip "Core idea"

    Recursion:

    Only attention is the tree in this question is a binary search tree, i.e. left node value is less than the parent node and the right node value is greater than the parent node.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        stack = [root]

        while stack:
            node = stack.pop()

            if node.val == val:
                return node

            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)

        return None
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    Only attention is the tree in this question is a binary search tree, i.e. left node value is less than the parent node and the right node value is greater than the parent node.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
            if not root:
                return None

            if root.val == val:
                return root
            if root.val > val:
                return self.searchBST(root.left, val)
            if root.val < val:
                return self.searchBST(root.right, val)
    ```

    Iteration:

    A typical iteration solution to the binary tree problem; no need to consider same-level nodes.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 15, 2026 |

---

[Back to Day 15 Binary Tree V](index.md) · [All LeetCode notes](../index.md)
