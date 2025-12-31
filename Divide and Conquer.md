# Divide and Conquer

> [!note] Description
> Divide and Conquer is an algorithmic paradigm. A typical Divide and Conquer algorithm solves a problem using following three steps:
> 1. **Divide**: Break the problem into smaller sub-problems.
> 2. **Conquer**: Solve the sub-problems recursively.
> 3. **Combine**: Combine the solutions of the sub-problems to get the solution of original problem.

## Examples
- Merge Sort
- Quick Sort
- Binary Search

## Complexity
Recurrence relations like $T(n) = aT(n/b) + f(n)$ are used to find complexity (Master Theorem).

## Implementation Structure
```python
def divide_and_conquer(P):
    if is_small(P):
        return solve(P)

    subproblems = divide(P)

    solutions = []
    for sp in subproblems:
        solutions.append(divide_and_conquer(sp))

    return combine(solutions)
```
