---
hide:
  - toc
---

# 226. Invert Binary Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/invert-binary-tree/)

!!! tip "Core idea"

    Recursion:

    simply swap the left node and the right node. traverse to the left node and the right node.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None

        stack = [root]

        while stack:
            node = stack.pop()
            node.left, node.right = node.right, node.left

            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    1. simply swap the left node and the right node.
    2. traverse to the left node and the right node.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
            if not root:
                return None

            root.left, root.right = root.right, root.left

            self.invertTree(root.left)
            self.invertTree(root.right)

            return root
    ```

    Iteration:

    1. In the while loop, pop the top node of the stack out, and swap the left node and the right node.
    2. add the current right node to the stack first and then current left node to the stack, so that in this way in the next iteration, we deal with the current left node previously right node) first.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |

---

[Back to Day 12 Binary Tree II](index.md) · [All Algorithm notes](../index.md)
