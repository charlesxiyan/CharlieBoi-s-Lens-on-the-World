---
hide:
  - toc
---

# 138. Copy List with Random Pointer

*Edited by Charles*

**Medium · Python · Graph · Two-pass HashMap**

## 1. Problem Description

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/copy-list-with-random-pointer/description/).

    Copy a linked list whose nodes have `val`, `next`, and `random` fields.
    The `random` pointer may reference any node in the list, including the node
    itself, or be `None`.

    Return the head of a deep copy. Preserve values and pointer relationships,
    but make every copied pointer refer to a copied node, never an original node.
    An empty list should produce `None`.

## 2. Approach and Core Idea

!!! tip "Create the nodes first; connect the pointers second"
    Store `copies[original] = clone`. Once all clones exist, translating either
    pointer is a dictionary lookup, even when it points forward in the list.

A `random` pointer can point to a node we have not reached yet. Creating and
connecting everything in a single straightforward left-to-right pass would
therefore require extra handling. Two passes separate these concerns cleanly.

**Why it works:** the first pass creates a one-to-one correspondence between
original and copied nodes. The second pass replaces each original pointer target
with its corresponding clone, reproducing both pointer structures.

Although we traverse a linked list, the extra `random` references make this a
small graph-cloning problem, so it fits the Graph bonus collection.

## 3. Algorithm Steps

1. If `head` is `None`, return `None`.
2. **First pass:** follow `next` from the head. Create one new node for each
   original and record it in `copies`.
3. **Second pass:** traverse the originals again. For each `current`, set:
   - `copies[current].next = copies.get(current.next)`
   - `copies[current].random = copies.get(current.random)`
4. Return `copies[head]`.

## 4. Hand Simulation

Use three distinct original nodes. A and B deliberately have the same value:

```text
next:    A(7) → B(7) → C(9) → None
random:  A → C, B → A, C → B
```

An index-based representation of this example is `[[7, 2], [7, 0], [9, 1]]`.
The indices describe the example; the function itself receives node objects.

**Pass 1: create only the nodes.**

| Original object | New object | Value |
| --- | --- | --- |
| A | A' | 7 |
| B | B' | 7 |
| C | C' | 9 |

All copied pointers are initially `None`.

**Pass 2: connect them using the mapping.**

| Current original | Copied `next` | Copied `random` |
| --- | --- | --- |
| A | `copies[B] = B'` | `copies[C] = C'` |
| B | `copies[C] = C'` | `copies[A] = A'` |
| C | `None` | `copies[B] = B'` |

The final result is:

```text
next:    A'(7) → B'(7) → C'(9) → None
random:  A' → C', B' → A', C' → B'
```

No copied pointer points to A, B, or C. Changing a cloned node's value will not
change the corresponding original node's value.

## 5. Important Implementation Details

### Dictionary keys must identify nodes, not their values

A and B both have value `7`, yet they are different nodes with different pointers.
A mapping such as `copies[7] = ...` would collapse them into one entry.

Use the **original node object as the key**. With the default node class, hashing
and equality distinguish objects by identity. If you define a custom equality
method, take care not to lose that identity-based behavior.

### Every possible pointer target exists before pass 2

The problem guarantees that a non-null `random` target is inside the list.
Following the `next` chain in pass 1 therefore creates a clone for every possible
target, including targets ahead of the current node.

### Null pointers need no special cloned node

`copies.get(None)` returns `None`, correctly preserving a missing `next` or
`random` pointer. All non-null pointer targets are present in the dictionary.

### Do not follow random pointers to drive the traversal

Move with `current = current.next`. The `next` chain ends at `None`, whereas
random pointers may create cycles or self-references. Both passes leave the
original list unchanged.

## 6. Time and Space Complexity

For a list of $n$ nodes:

| Resource | Complexity | Reason |
| --- | --- | --- |
| Time | $O(n)$ | Two linear passes with average constant-time dictionary operations. |
| Auxiliary space | $O(n)$ | One mapping entry per original node. |
| Output space | $O(n)$ | Exactly one new node per original node. |

A two-pass traversal is still linear: $O(2n)=O(n)$.

## 7. Complete Python Solution

LeetCode provides `Node`; this equivalent definition makes the example runnable
on its own as well.

```python
from typing import Optional


class Node:
    def __init__(self, val: int = 0, next=None, random=None):
        self.val = val
        self.next = next
        self.random = random


class Solution:
    def copyRandomList(self, head: Optional[Node]) -> Optional[Node]:
        if head is None:
            return None

        copies = {}
        current = head

        # Pass 1: create every cloned node.
        while current is not None:
            copies[current] = Node(current.val)
            current = current.next

        # Pass 2: translate both pointer targets into cloned targets.
        current = head
        while current is not None:
            clone = copies[current]
            clone.next = copies.get(current.next)
            clone.random = copies.get(current.random)
            current = current.next

        return copies[head]
```

## 8. Key Takeaways

- Separate object creation from pointer wiring.
- Node values are data; node identities are dictionary keys.
- A random-pointer cycle is harmless when traversal follows only `next`.
- Deep copying requires translating every reference into the copied structure.

---

[Back to Bonus Graph](index.md) · [All Algorithm notes](../index.md)
