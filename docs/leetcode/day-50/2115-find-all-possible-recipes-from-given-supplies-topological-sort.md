---
hide:
  - toc
---

# 2115. Find All Possible Recipes from Given Supplies (Topological Sort)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Use **Topological Sort + Kahn's Algorithm**.

        Each recipe depends on several ingredients.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    Use **Topological Sort + Kahn's Algorithm**.

    Each recipe depends on several ingredients.

    We can think of:

    ```text
    ingredient → recipe
    ```

    as a directed edge.

    For example:

    ```text
    flour ──→ bread
    yeast ──→ bread
    bread ──→ sandwich
    meat ───→ sandwich
    ```

    The key idea is:

    > Start with all available supplies. Whenever all ingredients of a recipe become available, we can make that recipe and use it as a new supply.
    >

    This is very similar to LeetCode 207 / 210.

    ## Graph

    Define:

    ```python
    graph[ingredient]
    ```

    as:

    > All recipes that require this ingredient.
    >

    We build the graph using:

    ```python
    for ingredient in ingredients[i]:
        graph[ingredient].append(recipe)
    ```

    For example:

    ```text
    recipes = ["bread", "sandwich"]

    ingredients = [
        ["yeast", "flour"],
        ["bread", "meat"]
    ]
    ```

    The graph becomes:

    ```text
    yeast → bread
    flour → bread
    bread → sandwich
    meat → sandwich
    ```

    So:

    ```python
    graph["yeast"] = ["bread"]
    graph["flour"] = ["bread"]
    graph["bread"] = ["sandwich"]
    graph["meat"] = ["sandwich"]
    ```

    ## Why `defaultdict(list)`?

    We define:

    ```python
    graph = defaultdict(list)
    ```

    If a key does not exist, `defaultdict(list)` automatically creates an empty list.

    For example:

    ```python
    graph["flour"].append("bread")
    ```

    If `"flour"` does not exist yet, Python automatically creates:

    ```python
    graph["flour"] = []
    ```

    and then appends `"bread"`.

    So:

    ```python
    graph["flour"] = ["bread"]
    ```

    This avoids writing:

    ```python
    if ingredient not in graph:
        graph[ingredient] = []
    ```

    Memory:

    > `defaultdict(list)` → missing key automatically gets `[]`.
    >

    ## Indegree

    Define:

    ```python
    indegree[recipe]
    ```

    as:

    > The number of ingredients that are still required to make this recipe.
    >

    Initially:

    ```python
    indegree[recipe] = len(ingredients[i])
    ```

    For example:

    ```text
    bread requires:
    - yeast
    - flour
    ```

    so:

    ```python
    indegree["bread"] = 2
    ```

    Similarly:

    ```text
    sandwich requires:
    - bread
    - meat
    ```

    so:

    ```python
    indegree["sandwich"] = 2
    ```

    ## Initialize the Queue

    Unlike normal Kahn's Algorithm, we do not start with nodes whose indegree is `0`.

    Instead, we already have some materials available:

    ```python
    supplies
    ```

    So initialize:

    ```python
    queue = deque(supplies)
    ```

    For example:

    ```python
    supplies = ["yeast", "flour", "meat"]
    ```

    then:

    ```text
    queue = [yeast, flour, meat]
    ```

    These are the materials we can use immediately.

    ## BFS / Kahn's Algorithm

    Each time we take an available material:

    ```python
    material = queue.popleft()
    ```

    We check which recipes require this material:

    ```python
    for recipe in graph[material]:
    ```

    Since this ingredient is now available, that recipe has one fewer missing ingredient:

    ```python
    indegree[recipe] -= 1
    ```

    If:

    ```python
    indegree[recipe] == 0
    ```

    then all ingredients required by this recipe are available.

    Therefore, we can make it:

    ```python
    res.append(recipe)
    ```

    ## Recipe Becomes a New Supply

    This is the most important part of the problem.

    After making a recipe, the recipe itself can be used as an ingredient for another recipe.

    Therefore:

    ```python
    queue.append(recipe)
    ```

    For example:

    ```text
    yeast + flour
          ↓
        bread
          +
         meat
          ↓
      sandwich
    ```

    Once `bread` is created, we add it to the queue:

    ```python
    queue.append("bread")
    ```

    Then `"bread"` can unlock `"sandwich"`.

    So the process continues until no more recipes can be created.

    ## Hand Simulation

    Suppose:

    ```python
    recipes = ["bread", "sandwich"]

    ingredients = [
        ["yeast", "flour"],
        ["bread", "meat"]
    ]

    supplies = ["yeast", "flour", "meat"]
    ```

    Initially:

    ```text
    indegree:

    bread    = 2
    sandwich = 2

    queue:
    [yeast, flour, meat]

    res:
    []
    ```

    ### Process `yeast`

    `yeast` is required by `bread`:

    ```text
    bread: 2 → 1
    ```

    State:

    ```text
    queue = [flour, meat]
    res   = []
    ```

    ### Process `flour`

    `flour` is also required by `bread`:

    ```text
    bread: 1 → 0
    ```

    Now all ingredients of `bread` are available.

    So:

    ```python
    res.append("bread")
    queue.append("bread")
    ```

    State:

    ```text
    queue = [meat, bread]
    res   = [bread]
    ```

    ### Process `meat`

    `meat` is required by `sandwich`:

    ```text
    sandwich: 2 → 1
    ```

    State:

    ```text
    queue = [bread]
    res   = [bread]
    ```

    ### Process `bread`

    `bread` is also required by `sandwich`:

    ```text
    sandwich: 1 → 0
    ```

    Now `sandwich` can be created:

    ```text
    queue = [sandwich]
    res   = [bread, sandwich]
    ```

    Finally, processing `sandwich` unlocks nothing else.

    Result:

    ```python
    ["bread", "sandwich"]
    ```

    ## Why Does This Handle Cycles?

    Suppose:

    ```text
    A requires B
    B requires A
    ```

    Then:

    ```text
    A → B
    B → A
    ```

    If neither `A` nor `B` is initially available in `supplies`, neither recipe can have its `indegree` reduced to `0`.

    Therefore, neither recipe will ever enter the queue.

    So recipes involved in an unresolved dependency cycle are automatically excluded.

    ## Connection to LeetCode 207 / 210

    The structure is almost the same as Course Schedule.

    ### LeetCode 210

    ```text
    prerequisite → course
    ```

    `indegree[course]` means:

    > How many prerequisites are still required?
    >

    ### LeetCode 2115

    ```text
    ingredient → recipe
    ```

    `indegree[recipe]` means:

    > How many ingredients are still required?
    >

    The major difference is the initial queue:

    ```text
    210:
    Start with courses whose indegree == 0.

    2115:
    Start with the given supplies.
    ```

    And in 2115, once a recipe is created:

    ```python
    queue.append(recipe)
    ```

    because that recipe becomes a new available material.

    ## Key Insight

    > Treat ingredients as prerequisites and recipes as courses.
    >

    Whenever an available material satisfies one dependency:

    ```python
    indegree[recipe] -= 1
    ```

    When:

    ```python
    indegree[recipe] == 0
    ```

    the recipe can be created and becomes a new supply.

    Memory:

    > **Supplies unlock recipes, and completed recipes become new supplies.**
    >

    ## Complexity

    Let:

    - `R` = number of recipes
    - `E` = total number of ingredient dependencies

    Building the graph takes:

    ```text
    O(E)
    ```

    Each dependency is processed at most once during BFS:

    ```text
    O(E)
    ```

    Therefore:

    ```text
    Time: O(R + E)
    Space: O(R + E)
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | August 27, 2026 |

## Python solution

```python
from collections import defaultdict, deque

class Solution:
    def findAllRecipes(self, recipes: List[str], ingredients: List[List[str]], supplies: List[str]) -> List[str]:
        graph = defaultdict(list)
        indegree = {}

        for i in range(len(recipes)):
            recipe = recipes[i]
            indegree[recipe] = len(ingredients[i])

            for ingredient in ingredients[i]:
                graph[ingredient].append(recipe)

        queue = deque(supplies)
        res = []

        while queue:
            material = queue.popleft()

            for recipe in graph[material]:
                indegree[recipe] -= 1

                if indegree[recipe] == 0:
                    res.append(recipe)
                    queue.append(recipe)

        return res
```

---

[Back to Day 50 Graph VIII](index.md) · [All Algorithm notes](../index.md)
