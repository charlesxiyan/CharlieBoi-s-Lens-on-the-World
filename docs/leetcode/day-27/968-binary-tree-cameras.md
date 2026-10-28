---
hide:
  - toc
---

# 968. Binary Tree Cameras

*Edited by Charles*

**Hard · Python · Greedy**

[Original problem](https://leetcode.com/problems/binary-tree-cameras/description/)

!!! tip "Core idea"

    There are only 4 states for each node and we decode these states by numbers: The basic logic is we need to check states from leaves up to the root. That is, we check left and right nodes so that we know the state of the current node.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        self.result = 0
        ### States:
        ### -1: None
        ### 0: Uncover
        ### 1: Cover
        ### 2: Camera
        self.traverse(root, False)

        return self.result

    def traverse(self, node, state):
        if not node:
            return -1

        left = self.traverse(node.left, True)
        right = self.traverse(node.right, True)

				# -1 -1, -1 0, -1 1, -1 2
				# 0 -1,   0 0,  0 1,  0 2
				# 1 -1,   1 0,  1 1,  1 2
				# 2 -1,   2 0,  2 1,  2 2
        if left == -1 and right == -1:
            if state:
                return 0
            self.result += 1
            return 2

				# if we must install camera
        if left == 0 or right == 0:
            self.result += 1
            return 2

				# if the current node is covered
        if left == 2 or right == 2:
            return 1

				# 不需要管子节点, 也没有子节点来管我 -> 让父节点来管我
        # Only cases left are left == 1 and right == 1;
        # left == -1 and right == 1; left == 1, and right == -1
        if state:
            return 0
        else:
            self.result += 1
            return 2
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. There are only 4 states for each node and we decode these states by numbers:
        1. -1: None
        2. 0: Uncover
        3. 1: Cover
        4. 2: Camera
    2. The basic logic is we need to check states from leaves up to the root. That is, we check left and right nodes so that we know the state of the current node.
        1. left and right are both None.
            1. If the current node is a leaf (it has a parent), return the state: 0 Uncover.
            2. if the current node is the root (it does not have a parent), return the state: 2, since there is no other nodes to cover the current node. Update the counter of cameras.
        2. either left or right is uncovered. Then, we need a camera to cover the uncovered child(ren). Return the state: 2. Update the counter of cameras.
        3. either left or right is set a camera. That is, the current node is under the coverage. Return the state: 1.
        4. the only left case: both left and right are covered.
            1. if the current node has a parent, return the state: 0, and logically the parent will be set camera.
            2. if the current node is the root, return the state: 2, since an extra camera is needed for the root. Update the counter of cameras.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 30, 2026 |

---

[Back to Day 27 Greedy Algorithm V](index.md) · [All Algorithm notes](../index.md)
