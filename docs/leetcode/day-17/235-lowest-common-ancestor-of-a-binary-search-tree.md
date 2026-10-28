---
hide:
  - toc
---

# 235. Lowest Common Ancestor of a Binary Search Tree

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recursion:

    Use the property of BST.

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
        cur = root

        while cur:
            if p.val < cur.val and q.val < cur.val:
                cur = cur.left
            elif p.val > cur.val and q.val > cur.val:
                cur = cur.right
            else:
                return cur
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    Use the property of BST.

    1. if p.val and q.val are less than the root.val, the nodes of p and q are on the left subtree.
    2. if p.val and q.val are greater than the root.val, the nodes of p and q are on the right subtree.
    3. all left cases are p.val ≤ root.val ≤ q.val or p.val ≥ root.val ≥ q.val.
    4. the constraints show that the root, p and q are always valid.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None

    class Solution:
        def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
            if p.val < root.val and q.val < root.val:
                return self.lowestCommonAncestor(root.left, p, q)

            if p.val > root.val and q.val > root.val:
                return self.lowestCommonAncestor(root.right, p, q)

            return root
    ```

    Iteration:

    Same idea as the recursion solution.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 18, 2026 |

---

[Back to Day 17 Binary Tree VII](index.md) · [All Algorithm notes](../index.md)
