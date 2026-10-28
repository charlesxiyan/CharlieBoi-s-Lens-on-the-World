---
hide:
  - toc
---

# 101. Symmetric Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/symmetric-tree/)

!!! tip "Core idea"

    Recursion:

    ultimate checks postorder traversal

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        dq = deque()
        dq.append(root.left)
        dq.append(root.right)

        while dq:
            left = dq.popleft()
            right = dq.popleft()

            if not left and not right:
                continue

            if not left and right:
                return False
            if left and not right:
                return False
            if left.val != right.val:
                return False

            dq.append(left.left)
            dq.append(right.right)
            dq.append(left.right)
            dq.append(right.left)

        return True
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    1. ultimate checks
        1. left: None v.s. right: not None → False
        2. left: not None v.s. right: None → False
        3. left: None v.s. right: None → True
        4. left.val ≠ right.val → False
        5. The reason why d is checked after a-c is left and right may be null. If d is checked in front of a, there will be errors since left.val or right.val does not exist.
    2. postorder traversal
        1. check children first, i.e. outer and inner. These two checks are actually recursions.
        2. check the parent. That is, the parents are equal only when the outer and the inner are True.
        3. For each individual node and its corresponding to-be-compared node, they are equal only when both the outer and the inner are True.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def isSymmetric(self, root: Optional[TreeNode]) -> bool:
            left, right = root.left, root.right
            return self.checkSymmetric(left, right)

        def checkSymmetric(self, left, right):
            if not left and right:
                return False
            if left and not right:
                return False
            if not left and not right:
                return True
            if left.val != right.val:
                return False

            # Check left/right
            outer = self.checkSymmetric(left.left, right.right)
            # Check right/left
            inner = self.checkSymmetric(left.right, right.left)
            # Check middle
            return outer and inner
    ```

    Iteration:

    1. The basic idea is still the same.
    2. Pay attention to the children nodes. Since, in every iteration, the queue pops out two elements, left.left and right.right should be appended to the queue in order and in pair; left.right and right.left should be appended in order and in pair as well.
    3. The solution uses queue. Similarly, stack also works in this question.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |

---

[Back to Day 12 Binary Tree II](index.md) · [All LeetCode notes](../index.md)
