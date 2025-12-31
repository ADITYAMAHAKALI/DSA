# Greedy Algorithm

> [!note] Description
> A greedy algorithm is an algorithmic paradigm that builds up a solution piece by piece, always choosing the next piece that offers the most obvious and immediate benefit.

## Intuition
The intuition is "live in the moment" or "take the best deal right now." Instead of looking at the big picture or planning steps ahead, a greedy algorithm makes the choice that looks best at the current step, hoping that these locally optimal choices will lead to a globally optimal solution. While this doesn't work for every problem (like shortest path with negative edges), it is very efficient for problems that satisfy the greedy choice property.

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
