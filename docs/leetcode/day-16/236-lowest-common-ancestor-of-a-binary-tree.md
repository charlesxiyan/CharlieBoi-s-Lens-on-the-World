---
hide:
  - toc
---

# 236. Lowest Common Ancestor of a Binary Tree

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Rewrite

    ```python
    if not root:
    return root
    if root == q or root == p:
    return root

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root == q or root == p or not root:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root

        return left if left else right
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Rewrite

        ```python
        if not root:
        		return root
        if root == q or root == p:
        		return root

        ############################################################

        if root == q or root == p or not root:
            return root
        ```

    2. Postorder Traversal: left - right - middle

        ```python
        # Left
        left = self.lowestCommonAncestor(root.left, p, q)
        # Right
        right = self.lowestCommonAncestor(root.right, p, q)
        # Middle
        # See below
        ```

    3. Middle:
        1. as long as neither left nor right is None, that means p and q respectively show up in left and right subtrees; the current node is the least common ancestor. This is the only way to find LCA.
        2. if the left is not None but the right is None, that means the backtrack path now is only for searching the left node and the other node has not showed up yet; thus, return left node all the way back.
        3. if the right is not None but the left is None, then the reasoning is similar to b.
        4. attention: consider q is the left child of p, the traversal stops at p and return p all the way back until the final return. The reason why it makes sense is the question itself declares 1. all nodes’ values are distinctive; 2. p and q always exist in the tree. This means any other path will only return None all the back until the root or meeting with p.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 16, 2026 |

---

[Back to Day 16 Binary Tree VI](index.md) · [All Algorithm notes](../index.md)
