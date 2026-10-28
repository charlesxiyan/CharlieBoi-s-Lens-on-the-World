---
hide:
  - toc
---

# 98. Validate Binary Search Tree

*Edited by Charles*

**Medium · Python · Binary Trees**

!!! tip "Core idea"

    Recursion:

    Traverse the whole tree by inorder traversal. Collect the node’s value in a list (left - middle - right), so that the list should be a monotonously increasing list if it is a BST.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    pre = -inf
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if root is None:
            return True

        if not self.isValidBST(root.left):
            return False

        if root.val <= self.pre:
            return False
        self.pre = root.val

        return self.isValidBST(root.right)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    1. Traverse the whole tree by inorder traversal.
    2. Collect the node’s value in a list (left - middle - right), so that the list should be a monotonously increasing list if it is a BST.
    3. The reason why we have a init function is that list.append(node.val) is used in function traverse. See the potential problem explained in [113. Path Sum II](../day-14/113-path-sum-ii.md).

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def __init__(self):
            self.list = []

        def isValidBST(self, root: Optional[TreeNode]) -> bool:
            self.list = []
            self.traverse(root)

            for i in range(1, len(self.list)):
                if self.list[i - 1] >= self.list[i]:
                    return False

            return True

        def traverse(self, node):
            if not node:
                return

            self.traverse(node.left)
            self.list.append(node.val)
            self.traverse(node.right)
    ```

    Iteration

    Reference to [94. Binary Tree Inorder Traversal](../day-11/94-binary-tree-inorder-traversal.md)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def isValidBST(self, root: Optional[TreeNode]) -> bool:
            stack = []
            cur = root
            pre = None

            while cur or len(stack) > 0:
                if cur:
                    stack.append(cur)
                    cur = cur.left
                else:
                    cur = stack.pop()
                    if pre and cur.val <= pre.val:
                        return False

                    pre = cur
                    cur = cur.right
            return True
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 15 Binary Tree V](index.md) · [All Algorithm notes](../index.md)
