---
hide:
  - toc
---

# 148. Sort List

*Edited by Charles*

**Medium · Python · Linked List · Merge Sort**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/sort-list/description/).

    Sort a singly linked list in ascending value order and return its head. An empty list should remain empty.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Split the list into two smaller lists, recursively sort both, and merge their
    sorted nodes. Merge sort works naturally with linked lists because merging
    only needs forward traversal and pointer updates.

    ## Algorithm Steps

    1. Return immediately for an empty list or a single node.
    2. Start `slow` at `head` and `fast` at `head.next`. Advance them by one and two
       nodes respectively until `fast` cannot take another two-node step.
    3. Save `right = slow.next`, then cut the link with `slow.next = None`.
    4. Recursively sort the left and right lists.
    5. Merge them using `dummy` and the output-tail pointer `curr`.
    6. Attach the remaining suffix when either side becomes empty.

    ## Hand Simulation

    For `4 → 2 → 1 → 3`, the first split puts `slow` at `2`:

    ```text
    [4, 2, 1, 3]
         /    \\
      [4, 2] [1, 3]
       / \\    / \\
     [4] [2] [1] [3]
       \\ /    \\ /
      [2, 4] [1, 3]
          \\   /
       [1, 2, 3, 4]
    ```

    The final merge compares `2` with `1` (take `1`), `2` with `3` (take `2`),
    and `4` with `3` (take `3`). The right side is then exhausted, so attach `4`.

    ## Important Implementation Details

    - Starting `fast` one node ahead leaves `slow` at the end of the left half.
      On a two-node list, the loop does not run and the split produces two singletons.
    - Save `slow.next` before clearing it. Cutting the link is essential: otherwise
      the recursive call on `head` could receive the original list again.
    - `dummy.next` gives the merged head without special-casing the first selection.
    - Every selection advances both the chosen input pointer and `curr`. Remaining
      suffixes are already sorted and can be attached in one pointer assignment.
    - Existing data nodes are reused, so this operation changes their `next` links.
    - Because the comparison is `<`, equal values are taken from the right list
      first. Value ordering is correct, but this implementation is not stable with
      respect to the original order of equal-valued node identities.
    - The supplied `import heapq` is retained; merge sort does not use it.

    **Why it works:** the base cases are sorted. Assuming each recursive half is
    sorted, its head is its smallest remaining element. Repeatedly selecting the
    smaller head creates a sorted output containing every node exactly once.

    ## Time and Space Complexity

    For $N$ nodes, splitting and merging cost $O(N)$ across each recursion level.
    Balanced splitting creates $O(\log N)$ levels, so time is $O(N\log N)$.
    The recursive stack uses $O(\log N)$ auxiliary space; each merge uses constant
    extra pointer storage. This top-down solution does not satisfy the follow-up's
    constant-space requirement. Bottom-up iterative merge sort is an alternative
    if that additional requirement is imposed.

    ## Key Takeaways

    A correct split must reduce both recursive subproblems. Reusing nodes keeps
    merging inexpensive, but recursive calls still contribute stack space.

## Python solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
import heapq

class Solution:
    def sortList(self, head: ListNode | None) -> ListNode | None:
        if not head or not head.next:
            return head

        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        right = slow.next
        slow.next = None

        left = self.sortList(head)
        right = self.sortList(right)

        return self.merge(left, right)

    def merge(self, left, right):
        dummy = ListNode()
        curr = dummy

        while left and right:
            if left.val < right.val:
                curr.next = left
                left = left.next
            else:
                curr.next = right
                right = right.next
            curr = curr.next

        if left:
            curr.next = left
        if right:
            curr.next = right

        return dummy.next
```

---

[Back to Bonus Linked List](index.md) · [All Algorithm notes](../index.md)
