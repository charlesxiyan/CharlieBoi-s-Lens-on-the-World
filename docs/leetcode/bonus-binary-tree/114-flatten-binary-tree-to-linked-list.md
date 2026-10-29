---
hide:
  - toc
---

# 114. Flatten Binary Tree to Linked List

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given the `root` of a binary tree, flatten the tree into a linked list **in-place**.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given the `root` of a binary tree, flatten the tree into a linked list **in-place**.

    The flattened tree should satisfy:

    ```text
    Every node's left child is None
    Every node's right child points to the next node
    ```

    The order of nodes should follow **preorder traversal**:

    ```text
    root → left → right
    ```

    ## Example

    Given:

    ```text
            1
           / \
          2   5
         / \   \
        3   4   6
    ```

    After flattening:

    ```text
    1
     \
      2
       \
        3
         \
          4
           \
            5
             \
              6
    ```

    Equivalent to:

    ```text
    1 → 2 → 3 → 4 → 5 → 6
    ```

    The main idea is:

    > First recursively flatten the left subtree and the right subtree, then connect them together.
    >

    For every node:

    ```text
            node
           /    \
        left    right
    ```

    we want to transform it into:

    ```text
    node
      \
      left
        \
        ...
          \
          right
    ```

    So the processing order is:

    ```text
    flatten(left)
    flatten(right)
    process current node
    ```

    This is a **postorder-style recursion**.

    For the current node:

    ```python
    self.flatten(root.left)
    self.flatten(root.right)
    ```

    After these two recursive calls, we can assume that both subtrees have already been flattened.

    For example:

    ```text
            1
           / \
          2   5
         / \   \
        3   4   6
    ```

    After recursively flattening the left subtree:

    ```text
    2
     \
      3
       \
        4
    ```

    After recursively flattening the right subtree:

    ```text
    5
     \
      6
    ```

    Now we only need to connect these two flattened lists.

    Before changing the pointers, save them:

    ```python
    left = root.left
    right = root.right
    ```

    We need to do this because we are about to modify:

    ```python
    root.left
    root.right
    ```

    If we do not save the original right subtree, we may lose it.

    The flattened tree must have:

    ```text
    left child = None
    ```

    So:

    ```python
    root.left = None
    ```

    Then move the flattened left subtree to the right:

    ```python
    root.right = left
    ```

    For example:

    ```text
    Before:

            1
           / \
          2   5

    After:

            1
             \
              2
    ```

    At this moment, the original right subtree `5` is temporarily disconnected, but we already saved it in:

    ```python
    right
    ```

    Now we need to attach the original right subtree after the flattened left subtree.

    Suppose the left subtree has already become:

    ```text
    2
     \
      3
       \
        4
    ```

    We need to find the last node:

    ```text
    4
    ```

    So:

    ```python
    cur = root

    while cur.right:
        cur = cur.right
    ```

    After the loop, `cur` points to the last node of the current right chain.

    Now connect:

    ```python
    cur.right = right
    ```

    For example:

    ```text
    Before:

    1
     \
      2
       \
        3
         \
          4

    right:

    5
     \
      6
    ```

    After:

    ```text
    1
     \
      2
       \
        3
         \
          4
           \
            5
             \
              6
    ```

    This gives the correct preorder order:

    ```text
    root → left subtree → right subtree
    ```

    Preorder traversal is:

    ```text
    root → left → right
    ```

    After flattening, we want:

    ```text
    root
      \
      flattened left subtree
            \
            flattened right subtree
    ```

    So the right pointer chain becomes exactly:

    ```text
    root → left subtree → right subtree
    ```

    which is preorder order.

    Consider:

    ```text
            1
           / \
          2   5
         / \   \
        3   4   6
    ```

    ## Process Node `2`

    Its left subtree is:

    ```text
    3
    ```

    Its right subtree is:

    ```text
    4
    ```

    After flattening both:

    ```text
    3
    ```

    and:

    ```text
    4
    ```

    Move left to right:

    ```text
    2
     \
      3
    ```

    Then attach the original right subtree:

    ```text
    2
     \
      3
       \
        4
    ```

    So the subtree rooted at `2` is finished.

    ## Process Node `5`

    Node `5` has only a right child:

    ```text
    5
     \
      6
    ```

    So it is already in the correct form.

    ## Process Node `1`

    Now the left subtree is:

    ```text
    2
     \
      3
       \
        4
    ```

    and the right subtree is:

    ```text
    5
     \
      6
    ```

    Move the left subtree to the right:

    ```text
    1
     \
      2
       \
        3
         \
          4
    ```

    Then attach the original right subtree:

    ```text
    1
     \
      2
       \
        3
         \
          4
           \
            5
             \
              6
    ```

    Done.

    For every node:

    ```text
            node
           /    \
        left    right
    ```

    First:

    ```text
    flatten(left)
    flatten(right)
    ```

    Then:

    ```text
    save left and right
    ```

    Then:

    ```text
    root.left = None
    root.right = left
    ```

    Then find the end of the right chain:

    ```text
    root → flattened left → ... → tail
    ```

    Finally:

    ```text
    tail.right = original right
    ```

    We write:

    ```python
    left = root.left
    right = root.right
    ```

    before changing the pointers.

    Because after:

    ```python
    root.right = left
    ```

    the original:

    ```python
    root.right
    ```

    would be overwritten.

    So we must save it first:

    ```python
    right = root.right
    ```

    Otherwise, the original right subtree would be lost.

    ## Time Complexity

    The recursive traversal visits every node.

    However, for every node, we may also search down the right chain:

    ```python
    while cur.right:
        cur = cur.right
    ```

    In the worst case, this repeated search can make the total time complexity:

    ```text
    O(n²)
    ```

    For example, when the tree is highly unbalanced.

    ## Space Complexity

    The recursion stack depends on the height of the tree:

    ```text
    O(h)
    ```

    For a balanced tree:

    ```text
    O(log n)
    ```

    For a completely skewed tree:

    ```text
    O(n)
    ```



    The recursive idea is:

    ```text
    1. Flatten left subtree
    2. Flatten right subtree
    3. Move left subtree to the right
    4. Find the end of the flattened left subtree
    5. Attach the original right subtree
    ```

    The structure changes from:

    ```text
            node
           /    \
        left    right
    ```

    to:

    ```text
    node
      \
      left
        \
        ...
          \
          right
    ```

    The most important thing to remember is:

    ```text
    Preorder order:
    root → left → right
    ```

    So after recursively flattening both subtrees, we simply connect them as:

    ```text
    root → flattened left → flattened right
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 10, 2026 |

## Python solution

```python
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        if not root:
            return

        self.flatten(root.left)
        self.flatten(root.right)

        left = root.left
        right = root.right

        root.left = None
        root.right = left

        cur = root

        while cur.right:
            cur = cur.right

        cur.right = right
```

---

[Back to Bonus Binary Tree](index.md) · [All Algorithm notes](../index.md)
