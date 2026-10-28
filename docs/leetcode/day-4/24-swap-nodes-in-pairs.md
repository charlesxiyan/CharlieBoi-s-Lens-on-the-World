---
hide:
  - toc
---

# 24. Swap Nodes in Pairs

*Edited by Charles*

**Medium · Python · LinkedList**

[Original problem](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

!!! tip "Core idea"

    This question requires to return a whole linked list, and thus a virtual head is needed. The pointer current should start from the virtual head, since the next and the [next.next](http://next.next) are two objects for swapping.

## Python solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(next = head)
        current = dummy

        while current.next and current.next.next:
            slow = current.next
            fast = current.next.next.next

            current.next = current.next.next
            current.next.next = slow
            slow.next = fast

            current = current.next.next

        return dummy.next
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. This question requires to return a whole linked list, and thus a virtual head is needed.
    2. The pointer current should start from the virtual head, since the next and the [next.next](http://next.next) are two objects for swapping.
    3. The while loop condition should be the to-be-swapped nodes are not None.
    4. The swap looks like
        1. VirtualHead → Node[0] → Node[1] → Node[2] → ……

                       |                     |                                         |

                 current           slow                                fast

        2. VirtualHead      Node[0] → Node[1] → Node[2] → ……

                        |_______________________|

        3. VirtualHead      Node[0] ← Node[1] → Node[2] → ……

                        |_______________________|

        4.                                   |—————>————|

            VirtualHead      Node[0] ← Node[1]      Node[2] → ……

                  |_______________________|

        5. VirtualHead → Node[1] → Node[0] → Node[2] → ……
    5. Besides the pointer current, we also need two pointers: slow and fast, since by redirecting the nodes, we need to find nodes that lost original references. For example, in step 4, Node[0] and Node[2] need alternative references, since without new references, we cannot find them after redirecting VirtualHead to Node[1] and redirecting Node[1] to Node[0].

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | January 4, 2026 |

---

[Back to Day 4 Linked List II](index.md) · [All Algorithm notes](../index.md)
