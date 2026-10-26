# Python: generators and lazy evaluation

A generator produces values on demand. It is useful when the input is large or
when a pipeline can process one item at a time.

```python title="squares.py" linenums="1"
def squares(limit: int):
    for number in range(limit):
        yield number * number


print(sum(squares(5)))  # 0 + 1 + 4 + 9 + 16 = 30
```

The generator keeps only its current state, so its auxiliary space is $O(1)$.
Collecting all results with `list(squares(n))` instead requires $O(n)$ space.

!!! note "One pass"
    A generator is exhausted after iteration. Create another generator if you
    need to iterate again.
