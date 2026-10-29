---
hide:
  - toc
---

# 513. Find Bottom Left Tree Value

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-bottom-left-tree-value/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Iteration:

        The most fabulous point of this solution is, when we iterate all nodes in the same level, the first valid/non-None node is the leftmost node so far.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Iteration:

    The most fabulous point of this solution is, when we iterate all nodes in the same level, the first valid/non-None node is the leftmost node so far.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    from collections import deque

    class Solution:
        def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
            dq = deque([root])
            result = 0

            while dq:
                n = len(dq)

                for i in range(n):
                    node = dq.popleft()

                    if i == 0:
                        result = node.val

                    if node.left:
                        dq.append(node.left)
                    if node.right:
                        dq.append(node.right)

            return result
    ```

    Recursion

    The tricky point in this solution is:

    1. Once we meet a node without any left and right children, we update maxDepth and result.
    2. Once we traverse all nodes, the maxDepth and result are definitely the final outcome.
    3. We do not need to worry that the maxDepth and result are overwritten by the right node with the same parent node, since maxDepth has been updated already and the right node has the same depth so it won’t trigger the update operation.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
            self.result = root.val
            self.maxDepth = 0

            self.traverse(root, 0)

            return self.result

        def traverse(self, node, depth):
            if not node.left and not node.right:
                if depth > self.maxDepth:
                    self.maxDepth = depth
                    self.result = node.val
                return

            if node.left:
                self.traverse(node.left, depth + 1)
            if node.right:
                self.traverse(node.right, depth + 1)
    ```

    ### BFS 解法

    题解链接 [https://leetcode.cn/problems/find-bottom-left-tree-value/solutions/2049776/bfs-wei-shi-yao-yao-yong-dui-lie-yi-ge-s-f34y/](https://leetcode.cn/problems/find-bottom-left-tree-value/solutions/2049776/bfs-wei-shi-yao-yao-yong-dui-lie-yi-ge-s-f34y/)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 15, 2026 |

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        q = deque([root])

        while q:
            node = q.popleft()
            # 先右后左
            if node.right: q.append(node.right)
            if node.left: q.append(node.left)

        return node.val
```

---

[Back to Day 14 Binary Tree IV](index.md) · [All Algorithm notes](../index.md)
