---
hide:
  - toc
---

# 145. Binary Tree Postorder Traversal

*Edited by Charles*

**Easy · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/binary-tree-postorder-traversal/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Reference to [144. Binary Tree Preorder Traversal](144-binary-tree-preorder-traversal.md)

    Original study notes follow, including intermediate snippets and earlier attempts.

    Reference to [144. Binary Tree Preorder Traversal](144-binary-tree-preorder-traversal.md)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
            if not root:
                return []

            stack = [root]
            result = []

            while stack:
                node = stack.pop()
                result.append(node.val)

                if node.left:
                    stack.append(node.left)
                if node.right:
                    stack.append(node.right)

            return result[::-1]
    ```

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
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:

        def preorder(node):
            if node is None:
                return

            # 写在这里是Preorder
            # res.append(node.val)

            preorder(node.left)

            # Inorder
            # res.append(node.val)

            preorder(node.right)

            # Postorder
            res.append(node.val)

        res = []
        preorder(root)
        return res
```

---

[Back to Day 11 Binary Tree I](index.md) · [All Algorithm notes](../index.md)
