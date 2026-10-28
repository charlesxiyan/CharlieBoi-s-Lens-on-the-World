---
hide:
  - toc
---

# 127. Word Ladder

*Edited by Charles*

**Hard · Python · Graph**

[Original problem](https://leetcode.com/problems/word-ladder/description/)

!!! tip "Core idea"

    This problem can be viewed as a **shortest path problem in an unweighted graph**.

## Python solution

```python
from collections import deque

class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        wordSet = set(wordList)

        if endWord not in wordSet:
            return 0

        queue = deque([(beginWord, 1)])

        while queue:
            word, step = queue.popleft()
            if word == endWord:
                return step

            for i in range(len(word)):
                for char in string.ascii_lowercase:
                    newWord = word[: i] + char + word[i + 1:]
                    if newWord in wordSet:
                        wordSet.remove(newWord)
                        queue.append((newWord, step + 1))

        return 0
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    This problem can be viewed as a **shortest path problem in an unweighted graph**.

    We can think of:

    ```text
    Each word = a vertex
    ```

    Two words are connected by an edge if they differ by exactly **one character**.

    For example:

    ```text
    hit → hot → dot → dog → cog
    ```

    Since every transformation has the same cost (`1` step), we should use **BFS** to find the shortest transformation sequence.

    > BFS is suitable because it explores the graph level by level, so the first time we reach `endWord`, we have found the shortest path.
    >

    ## Graph Representation

    We do not need to explicitly construct the entire graph.

    Instead, for each current word, we generate all possible neighboring words by replacing each character with `a-z`.

    For example:

    ```text
    word = "hit"
    ```

    For the first position:

    ```text
    ait
    bit
    cit
    ...
    zit
    ```

    For the second position:

    ```text
    hat
    hbt
    hct
    ...
    hzt
    ```

    For the third position:

    ```text
    hia
    hib
    hic
    ...
    hiz
    ```

    If a generated word exists in `wordList`, then it is a valid neighbor.

    ## Why Use a Set?

    Convert `wordList` into a set:

    ```python
    wordSet = set(wordList)
    ```

    We frequently need to check:

    ```python
    newWord in wordSet
    ```

    Using a set gives approximately:

    ```text
    O(1)
    ```

    lookup time.

    Also, once a word is visited, we remove it from the set:

    ```python
    wordSet.remove(newWord)
    ```

    This prevents the same word from being visited multiple times.

    Therefore, `wordSet` also works as our `visited` structure.

    ## BFS

    Initialize the queue with:

    ```python
    queue = deque([(beginWord, 1)])
    ```

    Each element contains:

    ```text
    (word, step)
    ```

    where `step` represents the number of words in the current transformation sequence.

    For example:

    ```text
    (hit, 1)
    ```

    means the current sequence contains only:

    ```text
    hit
    ```

    ## BFS Process

    For each word removed from the queue:

    ```python
    word, step = queue.popleft()
    ```

    If:

    ```python
    word == endWord
    ```

    we return:

    ```python
    step
    ```

    Otherwise, generate all possible words that differ by one character.

    For every valid unvisited word:

    ```python
    if newWord in wordSet:
    ```

    remove it from `wordSet`:

    ```python
    wordSet.remove(newWord)
    ```

    and add it to the queue:

    ```python
    queue.append((newWord, step + 1))
    ```

    ## Why BFS Instead of DFS?

    Consider:

    ```text
            B
           /
    A ----
           \
            C → D → E
    ```

    There may be many different transformation paths.

    DFS may explore a very long path before finding the destination.

    BFS explores by distance:

    ```text
    Level 1: distance 1
    Level 2: distance 2
    Level 3: distance 3
    ...
    ```

    Therefore, the first time BFS reaches `endWord`, that path must be the shortest transformation sequence.

    A useful rule is:

    ```text
    Reachability problem
    → DFS or BFS

    Shortest path in an unweighted graph
    → BFS
    ```

    ## Hand Simulation

    Consider:

    ```python
    beginWord = "hit"
    endWord = "cog"

    wordList = [
        "hot",
        "dot",
        "dog",
        "lot",
        "log",
        "cog"
    ]
    ```

    Initially:

    ```text
    queue = [(hit, 1)]
    ```

    ### Level 1

    Pop:

    ```text
    (hit, 1)
    ```

    Generate its valid neighbor:

    ```text
    hit → hot
    ```

    Queue becomes:

    ```text
    [(hot, 2)]
    ```

    ### Level 2

    Pop:

    ```text
    (hot, 2)
    ```

    Valid unvisited neighbors:

    ```text
    hot → dot
    hot → lot
    ```

    Queue:

    ```text
    [(dot, 3), (lot, 3)]
    ```

    ### Level 3

    From `dot`:

    ```text
    dot → dog
    ```

    From `lot`:

    ```text
    lot → log
    ```

    Queue:

    ```text
    [(dog, 4), (log, 4)]
    ```

    ### Level 4

    From `dog`:

    ```text
    dog → cog
    ```

    So:

    ```text
    queue = [(log, 4), (cog, 5)]
    ```

    Eventually we pop:

    ```text
    (cog, 5)
    ```

    Since:

    ```python
    word == endWord
    ```

    return:

    ```text
    5
    ```

    The shortest transformation sequence is:

    ```text
    hit → hot → dot → dog → cog
    ```

    which contains `5` words.

    ## Important Detail: Why Is the Answer 5 Instead of 4?

    There are four transformations:

    ```text
    hit → hot
    hot → dot
    dot → dog
    dog → cog
    ```

    However, the problem asks for the **number of words in the transformation sequence**, not the number of edges.

    Therefore:

    ```text
    hit → hot → dot → dog → cog
     1      2      3      4      5
    ```

    The answer is:

    ```text
    5
    ```

    This is why we initialize:

    ```python
    queue = deque([(beginWord, 1)])
    ```

    instead of starting with `0`.

    ## Important Detail: `endWord` Must Exist

    If:

    ```python
    endWord not in wordSet
    ```

    there is no valid transformation sequence because every transformed word must exist in `wordList`.

    Therefore:

    ```python
    if endWord not in wordSet:
        return 0
    ```

    ## Complexity Analysis

    Let:

    ```text
    N = number of words
    L = length of each word
    ```

    For each word, we try:

    ```text
    L positions × 26 characters
    ```

    Therefore, ignoring the cost of Python string construction, the main search work is approximately:

    ```text
    O(N × L × 26)
    ```

    With string construction included, it can be viewed as:

    ```text
    O(N × L²)
    ```

    since creating each candidate string costs `O(L)`.

    **Space Complexity:**

    ```text
    O(N)
    ```

    for the set and BFS queue, aside from storage for generated strings.

    ## Summary

    ```text
    1. Treat every word as a vertex.
    2. Two words have an edge if they differ by one character.
    3. The problem becomes an unweighted shortest-path problem.
    4. Use BFS.
    5. Generate neighbors by replacing each character with a-z.
    6. Use a set for fast lookup.
    7. Remove words from the set once visited.
    8. Store (word, step) in the queue.
    9. Return step when endWord is reached.
    ```

    The most important idea is:

    > **Word Ladder is a hidden unweighted graph shortest-path problem, so BFS is the natural solution.**
    >

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 20, 2026 |

---

[Back to Day 46 Graph IV](index.md) · [All Algorithm notes](../index.md)
