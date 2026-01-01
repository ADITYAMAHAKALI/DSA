# Dynamic Programming

> [!note] Description
> Dynamic Programming (DP) is a method for solving complex problems by breaking them down into simpler subproblems. It is applicable to problems exhibiting the properties of overlapping subproblems and optimal substructure.

## Intuition
The intuition behind Dynamic Programming is "don't repeat yourself." If you've already solved a subproblem (like calculating the 5th Fibonacci number), store that answer. When you need it again (like when calculating the 6th or 7th Fibonacci number), just look it up instead of recalculating it from scratch. This transforms exponential-time recursive solutions into efficient polynomial-time solutions.

## Core DP Patterns

- [[Linear DP]] (Fibonacci & Kadane's)
- [[Knapsack Patterns]] (0/1, Unbounded, Subset Sum)
- [[LCS and Strings]] (LCS, Edit Distance)
- [[DP on Grids]] (Unique Paths, Min Path Sum)
- [[Interval DP]] (MCM, Burst Balloons)
- [[DP on Trees]] (House Robber III, Max Path Sum)
- [[Bitmasking DP]] (TSP, Set Cover)
- [[Digit DP]] (Counting numbers with properties)
- [[State Machine DP]] (Stock Trading)

## Advanced DP Patterns

- [[Catalan Numbers]] (Unique BSTs, Parentheses)
- [[SOS DP]] (Sum Over Subsets)
- [[Binary Lifting]] (LCA, Path Queries)
- [[Insertion DP]] (Permutations)
- [[Probability and Expected Value DP]] (Knight Probability)
- [[DP on Graphs]] (Cheapest Flights, Count Paths)
- [[Advanced Digit DP]] (Complex Digit constraints)

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

## Related Algorithms
- **Shortest Path**: [[Bellman-Ford Algorithm]], [[Floyd-Warshall Algorithm]]
- **String Algorithms**: [[Knuth-Morris-Pratt (KMP) Algorithm]] (conceptually related to prefix property), [[Rabin-Karp Algorithm]]
- **Graph**: Many graph problems are solved using DP.
- **Recursion**: [[Divide and Conquer]] often overlaps with DP (DP is optimization of Divide and Conquer).
