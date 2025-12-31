# Dynamic Programming

> [!note] Description
> Dynamic Programming (DP) is a method for solving complex problems by breaking them down into simpler subproblems. It is applicable to problems exhibiting the properties of overlapping subproblems and optimal substructure.

## Intuition
The intuition behind Dynamic Programming is "don't repeat yourself." If you've already solved a subproblem (like calculating the 5th Fibonacci number), store that answer. When you need it again (like when calculating the 6th or 7th Fibonacci number), just look it up instead of recalculating it from scratch. This transforms exponential-time recursive solutions into efficient polynomial-time solutions.

## Key Concepts
- **Overlapping Subproblems**: Solutions to same subproblems are needed again and again.
- **Optimal Substructure**: Optimal solution of the given problem can be obtained by using optimal solutions of its subproblems.

## Approaches
- **Top-Down (Memoization)**: Store the result of a subproblem when it is solved for the first time.
- **Bottom-Up (Tabulation)**: Solve subproblems first and use their solutions to build up solutions to larger problems.

## Example: Fibonacci Sequence

### Top-Down (Memoization)
```python
memo = {}
def fib(n):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib(n-1) + fib(n-2)
    return memo[n]
```

### Bottom-Up (Tabulation)
```python
def fib_tab(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```
