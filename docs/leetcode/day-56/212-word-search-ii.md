---
hide:
  - toc
---

# 212. Word Search II

*Edited by Charles*

**Hard · Python · Trie · DFS · Backtracking**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/word-search-ii/description/).

    Find all dictionary words that can be traced on a nonempty rectangular board
    of lowercase letters. Each step moves up, down, left, or right. A cell cannot
    be reused within one word, and each found word should appear only once in the result.
    The dictionary contains unique, nonempty words.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Build one trie for the entire dictionary, then start board DFS from every cell.
    The trie checks whether the current board path is still a useful dictionary
    prefix. If the next board character is absent from `curr`, stop that branch.

    `path` records the letters consumed. `curr` tracks the corresponding trie node.
    Temporarily replacing a board letter with `'#'` prevents cell reuse; restoring
    it after exploring neighbors allows the cell to participate in other paths.

    ## Algorithm Steps

    1. Reset `self.res` and `self.root`, then insert every dictionary word.
    2. For each board position, call `dfs(i, j, self.root, '')`.
    3. Reject out-of-bounds coordinates and characters absent from the current trie node.
    4. Advance to the matching child and append its character to `path`.
    5. If `'#'` is present, record the word and delete that terminal marker.
    6. Mark the current board cell, search its four neighbors, and restore its letter.
    7. Return all recorded words after every starting position has been explored.

    ## Hand Simulation

    Use a custom board and dictionary:

    ```text
    board = [['a', 'b'],
             ['c', 'd']]
    words = ['ab', 'abd', 'ac', 'aba']
    ```

    The root has an `a` child. Under `a`, `b` is terminal and also leads to `d` and
    `a`; `c` is terminal.

    1. Start at `(0, 0)`: consume `a`, set `path = 'a'`, and mark that board cell.
    2. DFS visits neighbors in the code's order: up, down, left, right. Down reaches
       `c`, records `ac`, removes its terminal marker, and restores `c` on return.
    3. Right reaches `b`, records `ab`, and removes only its terminal marker.
    4. Continue from `b` to `d` below it: record `abd`, then restore `d`.
    5. The path `aba` would need to reuse the original `a` cell. It is still marked,
       so that branch is rejected.
    6. Restore `b`, then restore `a`. Other starts have no matching root edge.

    The returned list is `['ac', 'ab', 'abd']` for this traversal order. The board
    ends exactly as it began. Result order is not part of the problem requirement.

    ## Important Implementation Details

    ### Finding a word does not end the search

    `ab` can be both a complete word and a prefix of `abd`. After appending `ab`,
    continue exploring the current node's children. Returning immediately would
    miss longer words with the same prefix.

    ### Deleting a terminal marker deduplicates results

    A word may be reachable from several starting positions or board paths.
    `del curr['#']` ensures it is added only on its first discovery. This deletion
    is global and intentional: unlike board marks, it is **not restored**.
    Only word-end metadata is removed; longer-word branches remain intact.

    ### The two uses of `'#'` depend on operation order

    The trie uses `'#'` as a word-end key; the board uses it as a visited marker.
    In this implementation, any terminal marker at the current trie node is deleted
    **before that node is passed to neighboring DFS calls**. Thus a visited board
    cell containing `'#'` fails `if char not in curr` and is rejected. The root has
    no terminal marker because dictionary words are nonempty.

    This explains why the supplied code needs no separate visited check. Keeping
    terminal markers while otherwise leaving the code unchanged would break that
    invariant and could attempt to follow a Boolean value as a child.

    ### Board restoration and string handling

    The board is shared mutable state, so `board[i][j] = char` is essential after
    all four recursive calls. Each recursive frame keeps its own `path` string:
    `path += char` creates a longer string, so no explicit string backtracking is needed.

    The `search` helper is preserved from the supplied implementation but is not
    called by `findWords`. DFS already carries its trie position; restarting a
    full-word lookup at every step would repeat work. After a successful search,
    terminal markers for found words have been removed, so this trie no longer
    represents an unchanged dictionary for subsequent `search` calls. A new
    `findWords` call rebuilds it.

    **Why it works:** each explored path follows adjacent distinct board cells and
    valid trie prefixes. A terminal node certifies a dictionary word. DFS explores
    all possible valid continuations, and deleting a terminal marker suppresses
    only duplicate outputs, not the paths needed for longer words.

    ## Time and Space Complexity

    Let the board have $M\times N$ cells, let $S$ be the sum of dictionary word
    lengths, and set $K=\min(L,MN)$, where $L$ is the longest word length.
    Building the trie takes expected $O(S)$ time and space.

    From each starting cell, there are at most four first moves and at most three
    subsequent choices because the immediately previous cell cannot be reused.
    Ignoring string-copy costs gives a conservative search bound of $O(MN\cdot3^K)$.
    Prefix checks usually prune many of these paths.

    For **this Python implementation**, creating `path + char` can copy up to $K$
    characters, so a safe time bound is

    $$O(S+MN\cdot K\cdot3^K).$$

    There are $O(K)$ recursive frames, but their retained prefix strings can occupy
    $1+2+\cdots+K=O(K^2)$ characters along a path. Auxiliary space excluding the
    returned words is therefore $O(S+K^2)$. The board marking uses no separate
    visited matrix. Output storage is proportional to the total size of found words.

    ## Key Takeaways

    The trie shares dictionary-prefix work across board searches. Backtracking
    restores board cells, while terminal deletion permanently records that a word
    has already been reported. These two kinds of state need different treatment.

    An optional optimization is to remove exhausted trie branches, but the supplied
    solution only removes terminal markers. Its code is retained as written.

## Python solution

```python
class Solution:
    def findWords(self, board: list[list[str]], words: list[str]) -> list[str]:
        self.res = []
        self.root = {}

        m, n = len(board), len(board[0])

        for word in words:
            self.insert(word)

        def dfs(i, j, curr, path):
            if i < 0 or i >= m or j < 0 or j >= n:
                return

            char = board[i][j]

            if char not in curr:
                return

            curr = curr[char]
            path += char

            if '#' in curr:
                self.res.append(path)
                del curr['#']

            # each cell can only be used once
            board[i][j] = '#'

            dfs(i - 1, j, curr, path)
            dfs(i + 1, j, curr, path)
            dfs(i, j - 1, curr, path)
            dfs(i, j + 1, curr, path)

            board[i][j] = char

        for i in range(m):
            for j in range(n):
                dfs(i, j, self.root, '')

        return self.res

    def insert(self, word):
        curr = self.root
        for char in word:
            if char not in curr:
                curr[char] = {}
            curr = curr[char]
        curr['#'] = True

    def search(self, word):
        curr = self.root
        for char in word:
            if char not in curr:
                return False
            curr = curr[char]
        return '#' in curr
```

---

[Back to Day 56](index.md) · [All Algorithm notes](../index.md)
