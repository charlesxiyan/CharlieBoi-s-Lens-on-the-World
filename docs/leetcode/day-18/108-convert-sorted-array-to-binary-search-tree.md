---
hide:
  - toc
---

# 108. Convert Sorted Array to Binary Search Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recursion:3

    The core idea is to just focus on creating the current node and letting the creation of the left and right children to the next recursions. The terminal condition is the input list is empty.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if len(nums) == 0:
            return None

        idx = len(nums) // 2

        root = TreeNode(val = nums[idx])
        root.left = self.sortedArrayToBST(nums[:idx])
        root.right = self.sortedArrayToBST(nums[idx + 1:])

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:3

    The core idea is to just focus on creating the current node and letting the creation of the left and right children to the next recursions. The terminal condition is the input list is empty.

    The list is sorted, and thus we partition the list in every recursion by the middle [704. Binary Search](https://app.notion.com/p/704-Binary-Search-2ec3d95ea5be804fafb8f2ea6e69db67?pvs=21) .

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 19, 2026 |

---

[Back to Day 18 Binary Tree VIII](index.md) · [All Algorithm notes](../index.md)
