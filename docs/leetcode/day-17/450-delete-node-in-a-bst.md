---
hide:
  - toc
---

# 450. Delete Node in a BST

*Edited by Charles*

**Medium · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/delete-node-in-a-bst/description/)

!!! tip "Core idea"

    Search recursively for the key. Handle zero or one child directly; with two children, reconnect the subtrees while preserving BST order.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None

        if root.val == key:
            if not root.left and not root.right:
                return None
            elif root.left and not root.right:
                return root.left
            elif not root.left and root.right:
                return root.right
            else:
                cur = root.right
                while cur.left:
                    cur = cur.left
                cur.left  = root.left

                return root.right

        if root.val < key:
            root.right = self.deleteNode(root.right, key)
        elif root.val > key:
            root.left = self.deleteNode(root.left, key)

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    5 cases:

    1. No such a node with this key value. Return None. (Nothing happens, i.e. no node change)
    2. There exists such a node.
        1. this node is a leaf, i.e. left and right children are None. Return None to the last recursion, i.e. redefine the child node as None (Delete the current node!).
        2. the node only has a left subtree. Return the left child node, i.e. redefine the child node as the current’s left child node.
        3. the node only has a right subtree. Return the right child node, i.e. redefine the child node as the current’s right child node.
        4. the node has both subtrees. Recall [701. Insert into a Binary Search Tree](701-insert-into-a-binary-search-tree.md). Delete the current node, connect the current node’s right subtree with the the node in the last recursion and zip the current node’s left subtree to the leftmost leaf of the right subtree. (Use the property of BST!)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 18, 2026 |

---

[Back to Day 17 Binary Tree VII](index.md) · [All Algorithm notes](../index.md)
