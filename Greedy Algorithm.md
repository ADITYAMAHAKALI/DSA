# Greedy Algorithm

> [!note] Description
> A greedy algorithm is an algorithmic paradigm that builds up a solution piece by piece, always choosing the next piece that offers the most obvious and immediate benefit.

## Characteristics
- **Greedy Choice Property**: A global optimum can be arrived at by selecting a local optimum.
- **Optimal Substructure**: An optimal solution to the problem contains an optimal solution to subproblems.

## Example: Activity Selection Problem
```python
def print_max_activities(s, f):
    n = len(f)
    print("Following activities are selected:")

    # The first activity is always selected
    i = 0
    print(i, end=' ')

    for j in range(1, n):
        if s[j] >= f[i]:
            print(j, end=' ')
            i = j

# s[] = start times, f[] = finish times (sorted)
s = [1, 3, 0, 5, 8, 5]
f = [2, 4, 6, 7, 9, 9]
print_max_activities(s, f)
```
