---
hide:
  - toc
---

# 208. Implement Trie (Prefix Tree)

*Edited by Charles*

**Medium · Python · Trie · Nested dictionaries**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/implement-trie-prefix-tree/description/).

    Build a structure supporting word insertion, exact-word lookup, and prefix lookup.
    `search(word)` must distinguish a complete inserted word from a prefix of one.
    Inputs contain lowercase English letters.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Each dictionary represents a prefix. A letter key leads to another dictionary;
    `'#': True` means that a complete inserted word ends at the current node.
    `self.root` represents the empty prefix. Words with the same prefix reuse its path.

    The key distinction is the final check: `search` requires `'#'`, whereas
    `startsWith` only requires that the entire prefix path exists.

    ## Algorithm Steps

    1. For insertion, start `curr` at the root and process each `char`.
    2. Create an empty dictionary only when that character is missing.
    3. Move `curr` to its child dictionary and mark the final node with `'#'`.
    4. For either lookup, return `False` immediately if an edge is missing.
    5. After exact lookup, test the terminal marker; after prefix lookup, return `True`.

    ## Hand Simulation

    Insert `cat`, then `car`. The resulting structure is:

    ```text
    root
    └── c
        └── a
            ├── t
            │   └── #: True
            └── r
                └── #: True
    ```

    | Operation | Result | Reason |
    | --- | --- | --- |
    | `search('cat')` | `True` | The path exists and its last node has `'#'`. |
    | `search('ca')` | `False` | The path exists, but no word ends there yet. |
    | `startsWith('ca')` | `True` | The prefix path exists. |
    | `insert('ca')` | — | Add `'#'` at the existing `a` node. |
    | `search('ca')` | `True` | That node now marks a complete word. |
    | `startsWith('cb')` | `False` | There is no `b` edge after `c`. |

    ## Important Implementation Details

    - `curr = curr[char]` changes which dictionary the local variable references;
      it does not copy a node or move `self.root`.
    - The terminal marker can coexist with children. A short word can be a prefix
      of a longer word without losing either one.
    - Reinserting a word simply writes the same terminal marker again.
    - `'#'` is safe as a marker because it cannot occur in the lowercase input alphabet.

    **Why it works:** after processing each character, `curr` is the node for exactly
    that processed prefix. Missing edges prove that prefix was never inserted;
    reaching a terminal marker proves the whole word was inserted.

    ## Time and Space Complexity

    For a word or prefix of length $L$, each operation takes expected $O(L)$ time
    under average constant-time dictionary access. Insertion creates at most $L$
    nodes, so it uses $O(L)$ additional space in the worst case. Lookup uses $O(1)$
    auxiliary space. Across inserted words with total length $S$, storage is $O(S)$
    (and can be smaller because prefixes are shared).

    ## Key Takeaways

    A trie stores prefixes structurally. A path proves prefix membership;
    a terminal marker proves exact-word membership.

## Python solution

```python
class Trie:
    def __init__(self):
        self.root = {}

    def insert(self, word: str) -> None:
        curr = self.root
        for char in word:
            if char not in curr:
                curr[char] = {}
            curr = curr[char]
        curr['#'] = True

    def search(self, word: str) -> bool:
        curr = self.root
        for char in word:
            if char not in curr:
                return False
            curr = curr[char]
        return '#' in curr

    def startsWith(self, prefix: str) -> bool:
        curr = self.root
        for char in prefix:
            if char not in curr:
                return False
            curr = curr[char]
        return True


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

---

[Back to Day 56](index.md) · [All Algorithm notes](../index.md)
