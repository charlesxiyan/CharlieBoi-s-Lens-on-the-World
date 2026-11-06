---
hide:
  - toc
---

# 23. Merge k Sorted Lists

*Edited by Charles*

**Hard · Python · Linked List · Min-Heap**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/merge-k-sorted-lists/description/).

    Merge a collection of ascending sorted linked lists into one sorted linked list. The collection and any individual list may be empty.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Keep the first unconsumed node of every nonempty input list in `minHeap`.
    The smallest remaining node across all lists must be one of those frontier
    nodes. Pop it, append it to the output, and replace it with its successor.

    Each heap entry is `(node.val, list_index, node)`.

    ## Algorithm Steps

    1. Create an empty heap and a dummy output head.
    2. Push each nonempty list's head, recording its index from `enumerate(lists)`.
    3. Pop the smallest tuple and link its `node` after `curr`.
    4. Advance `curr` to that node.
    5. If the popped node has a successor, push that successor with the same index.
    6. Continue until the heap is empty, then return `dummy.next`.

    ## Hand Simulation

    Use `lists = [[1, 4], [1, 3], [2]]`.

    | Action | Output so far | Remaining frontier as `(value, index)` |
    | --- | --- | --- |
    | Initialize | `[]` | `(1,0), (1,1), (2,2)` |
    | Pop `(1,0)`, push `(4,0)` | `[1]` | `(1,1), (2,2), (4,0)` |
    | Pop `(1,1)`, push `(3,1)` | `[1,1]` | `(2,2), (3,1), (4,0)` |
    | Pop `(2,2)` | `[1,1,2]` | `(3,1), (4,0)` |
    | Pop `(3,1)` | `[1,1,2,3]` | `(4,0)` |
    | Pop `(4,0)` | `[1,1,2,3,4]` | Empty |

    The frontier is shown sorted for readability; a heap's backing array is not
    necessarily fully sorted.

    ## Important Implementation Details

    ### Why store the list index?

    Python compares tuples lexicographically. It first compares values; when values
    tie, it compares list indices. This avoids trying to order `ListNode` objects.
    At most one node from any particular list is in the heap at a time, so two live
    entries cannot have the same list index. The index is therefore sufficient
    as the tie-breaker even when a list contains repeated values.

    ### Why only one node per list?

    Every later node in a sorted list is at least as large as its first unconsumed
    node. A later node cannot beat that frontier. After the frontier is popped,
    its successor becomes the next relevant candidate.

    ### Pointer updates and empty inputs

    `curr.next = node` reuses the original node. Updating `curr` changes only the
    local tail reference, so `node.next` still identifies the successor to enqueue.
    The input lists' links are modified as the output is assembled. Standard inputs
    are separate, acyclic linked lists. If there are no nodes, the heap stays empty
    and `dummy.next` is `None`.

    **Why it works:** the heap contains every list's smallest unconsumed node. Its
    minimum is thus the global minimum remaining node. Repeating that step outputs
    all nodes once in sorted order.

    ## Time and Space Complexity

    Let $k$ be the number of input lists, $h$ the number of nonempty lists, and $N$
    the total node count. Scanning input heads costs $O(k)$, and the heap holds at
    most $h$ entries. Each node is pushed and popped once, giving
    $O(k+N\log(h+1))$ time, including the zero/one-list edge cases.
    This is commonly summarized as $O(N\log k)$ when $k\ge2$ and the input-head scan
    is absorbed. The heap uses $O(h)$ extra space, at most $O(k)$.

    ## Key Takeaways

    A heap merges multiple sorted streams by tracking their frontiers. The tuple's
    index resolves equal values safely without comparing node objects.

## Python solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

import heapq

class Solution:
    def mergeKLists(self, lists: list[ListNode | None]) -> ListNode | None:
        minHeap = []
        dummy = ListNode(0, None)
        curr = dummy

        for i, node in enumerate(lists):
            if node:
                heapq.heappush(minHeap, (node.val, i, node))

        while minHeap:
            val, idx, node = heapq.heappop(minHeap)

            curr.next = node
            curr = curr.next

            if node.next:
                heapq.heappush(minHeap, (node.next.val, idx, node.next))

        return dummy.next
```

---

[Back to Bonus Linked List](index.md) · [All Algorithm notes](../index.md)
