---
hide:
  - toc
---

# 701. Insert into a Binary Search Tree

*Edited by Charles*

**Medium · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/insert-into-a-binary-search-tree/)

!!! tip "Core idea"

    Recursion:

    The core idea of the solution is, in BST, any node can be inserted as a leaf of the tree. For example, [4, 3, 7] and insert 5.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val = val)

        cur = root

        while cur:
            pre = cur
            if cur.val < val:
                cur = cur.right
            elif cur.val > val:
                cur = cur.left

        node = TreeNode(val = val)

        if val < pre.val:
            pre.left = node
        else:
            pre.right = node

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    The core idea of the solution is, in BST, any node can be inserted as a leaf of the tree. For example, [4, 3, 7] and insert 5.

    1. The first idea is to insert between 3 and 7, make 5 as the right node of 3 and 7 as the right node of 5.
    2. The second idea is to insert a left leaf to 7.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
            if not root:
                return TreeNode(val = val)

            if root.val < val:
                root.right = self.insertIntoBST(root.right, val)
            if root.val > val:
                root.left = self.insertIntoBST(root.left, val)

            return root
    ```

    Iteration:

    The same idea as the recursion.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 18, 2026 |

---

[Back to Day 17 Binary Tree VII](index.md) · [All Algorithm notes](../index.md)
