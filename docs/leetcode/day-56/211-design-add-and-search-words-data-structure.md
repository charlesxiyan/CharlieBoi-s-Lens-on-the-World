---
hide:
  - toc
---

# 211. Design Add and Search Words Data Structure

*Edited by Charles*

**Medium · Python · Trie · DFS · Wildcard matching**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/design-add-and-search-words-data-structure/description/).

    Support adding lowercase words and searching for a matching whole word.
    In a search pattern, `.` matches exactly one arbitrary letter. It does not match
    zero letters or an arbitrary-length sequence.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Insertion uses the same nested-dictionary trie as problem 208. Search becomes
    DFS because a literal character identifies one child, but a dot may match
    any letter child. The state `dfs(idx, curr)` means: the first `idx` pattern
    characters have already been matched, and `curr` is the node reached.

    ## Algorithm Steps

    1. `addWord` creates missing letter nodes and adds `'#'` at the end.
    2. Search begins with `dfs(0, self.root)`.
    3. If the entire pattern is consumed, succeed only if the current node is terminal.
    4. For a literal, follow its child if present; otherwise fail.
    5. For `.`, recursively try every letter child, advancing `idx` by one.
    6. Return `True` as soon as a branch succeeds; return `False` only after all fail.

    ## Hand Simulation

    Insert `cat`, `car`, and `dog`, then search for `c.r`.

    | State | Action |
    | --- | --- |
    | `idx = 0`, root | Literal `c`: follow the `c` child. |
    | `idx = 1`, prefix `c` | Dot: try each letter child; here only `a` exists. |
    | `idx = 2`, prefix `ca` | Literal `r`: follow the `r` child. |
    | `idx = 3`, prefix `car` | Pattern is consumed and `'#'` exists: return `True`. |

    For `.og`, the `c` branch fails because it has no `o` child. The search must
    continue to the `d` branch, which reaches `dog`. Returning `False` immediately
    on the first failed child would miss this match.

    Searching for `c.` returns `False`: it reaches `ca`, but only longer words
    end below that node. The pattern must match a complete word of the same length.

    ## Important Implementation Details

    - Skip the key `'#'` when branching. Its value is `True`, not a child dictionary,
      and it does not represent a letter a dot can consume.
    - Every recursive call advances `idx` by one. A dot consumes exactly one letter.
    - A successful child ends the search, but a failed child does not rule out siblings.
    - The search only reads the trie, so no restoration or visited set is needed.
    - The recursion explores a tree of prefixes, not a graph with cycles.

    **Why it works:** a literal enumerates its only possible matching edge. A dot
    enumerates every allowed matching edge. The base case accepts precisely the
    complete stored words, so DFS succeeds exactly when some inserted word matches.

    ## Time and Space Complexity

    Let $L$ be the pattern length, $d$ its number of dots, and $S$ the total number
    of inserted characters. Insertion takes expected $O(L)$ time for a length-$L$
    word and adds at most $O(L)$ space.

    Without dots, search takes $O(L)$ time. With dots, a conservative bound is
    $O(L\cdot26^d)$, since each dot can branch to at most 26 letters; only existing
    trie branches are actually explored. Equivalently, time is proportional to
    visited nodes and inspected child edges, bounded by the stored trie size.
    The current problem permits at most two dots, although this implementation
    also handles more. The recursive stack uses $O(L)$ space; trie storage is $O(S)$.

    ## Key Takeaways

    Wildcard matching turns a single trie walk into a branching search. Keep the
    terminal check, skip metadata keys, and exhaust alternatives before returning failure.

## Python solution

```python
class WordDictionary:

    def __init__(self):
        self.root = {}

    def addWord(self, word: str) -> None:
        curr = self.root

        for char in word:
            if char not in curr:
                curr[char] = {}
            curr = curr[char]
        curr['#'] = True

    def search(self, word: str) -> bool:
        def dfs(idx, curr):
            if idx == len(word):
                return '#' in curr

            char = word[idx]

            if char != '.':
                if char not in curr:
                    return False
                return dfs(idx + 1, curr[char])
            else:
                for key in curr:
                    if key == '#':
                        continue
                    if dfs(idx + 1, curr[key]):
                        return True
                return False

        return dfs(0, self.root)


# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```

---

[Back to Day 56](index.md) · [All Algorithm notes](../index.md)
