> [!note] Description
> DP on Grids involves finding optimal paths or calculating properties in a 2D grid where movement is restricted (usually down and right).

## 1. Unique Paths

### Intuition
A robot is located at the top-left corner of a `m x n` grid. It can only move either down or right at any point in time. The robot wants to reach the bottom-right corner. How many possible unique paths are there?

For any cell `(i, j)`, the robot could have arrived from `(i-1, j)` (top) or `(i, j-1)` (left). So `paths(i, j) = paths(i-1, j) + paths(i, j-1)`.

### Top-Down (Memoization)
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        memo = {}

        def dp(r, c):
            if r == 0 and c == 0:
                return 1
            if r < 0 or c < 0:
                return 0
            if (r, c) in memo:
                return memo[(r, c)]

            memo[(r, c)] = dp(r - 1, c) + dp(r, c - 1)
            return memo[(r, c)]

        return dp(m - 1, n - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0] * n for _ in range(m)]

        for i in range(m):
            dp[i][0] = 1
        for j in range(n):
            dp[0][j] = 1

        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]

        return dp[m - 1][n - 1]
```

### Performance Optimization
**State Compression:**
We only need the previous row (or even just the current row being updated in place).
**Math Approach:**
This is a combinatorics problem. Total steps = $(m-1) + (n-1)$. We need to choose $m-1$ moves down (or $n-1$ right). Result is $\binom{m+n-2}{m-1}$.

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Combinatorics Solution
        import math
        return math.comb(m + n - 2, m - 1)
```

### Complexity
- **Time:** $O(M \cdot N)$ (DP) or $O(\min(M, N))$ (Math)
- **Space:** $O(N)$ (DP optimized) or $O(1)$ (Math)

---

## 2. Minimum Path Sum

### Intuition
Given a `m x n` grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path. You can only move down or right.
Similar to Unique Paths, but instead of summing ways, we take the `min` of the costs from top or left.
`dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`

### Top-Down (Memoization)
```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        memo = {}

        def dp(r, c):
            if r == 0 and c == 0:
                return grid[0][0]
            if r < 0 or c < 0:
                return float('inf')
            if (r, c) in memo:
                return memo[(r, c)]

            memo[(r, c)] = grid[r][c] + min(dp(r - 1, c), dp(r, c - 1))
            return memo[(r, c)]

        return dp(m - 1, n - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        dp = [[0] * n for _ in range(m)]

        dp[0][0] = grid[0][0]

        # Fill first col
        for i in range(1, m):
            dp[i][0] = dp[i - 1][0] + grid[i][0]
        # Fill first row
        for j in range(1, n):
            dp[0][j] = dp[0][j - 1] + grid[0][j]

        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = grid[i][j] + min(dp[i - 1][j], dp[i][j - 1])

        return dp[m - 1][n - 1]
```

### Performance Optimization
**Space Optimization:**
Can be done in $O(N)$ space using a 1D array, or $O(1)$ space by modifying the input `grid` in-place.

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])

        for i in range(1, m):
            grid[i][0] += grid[i - 1][0]
        for j in range(1, n):
            grid[0][j] += grid[0][j - 1]

        for i in range(1, m):
            for j in range(1, n):
                grid[i][j] += min(grid[i - 1][j], grid[i][j - 1])

        return grid[m - 1][n - 1]
```

### Complexity
- **Time:** $O(M \cdot N)$
- **Space:** $O(1)$ (In-place)

---

## 3. Maximal Square

### Intuition
Given an `m x n` binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.
If `matrix[i][j] == '1'`, then the size of the square ending at `(i, j)` is limited by the smallest square ending at `(i-1, j)`, `(i, j-1)`, and `(i-1, j-1)`.
`dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1`.

### Top-Down (Memoization)
```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if not matrix: return 0
        m, n = len(matrix), len(matrix[0])
        memo = {}
        max_side = 0

        def dp(r, c):
            nonlocal max_side
            if r < 0 or c < 0:
                return 0
            if (r, c) in memo:
                return memo[(r, c)]

            right = dp(r, c - 1)
            down = dp(r - 1, c)
            diag = dp(r - 1, c - 1)

            if matrix[r][c] == '1':
                memo[(r, c)] = 1 + min(right, down, diag)
                max_side = max(max_side, memo[(r, c)])
            else:
                memo[(r, c)] = 0

            return memo[(r, c)]

        dp(m - 1, n - 1)
        # Need to call for all cells because max square might not end at m-1, n-1
        # The recursion above is insufficient as written if only called once.
        # Top-down typically iterates all cells to start recursion.

        # Better recursive structure:
        memo = {}
        ans = 0
        for i in range(m):
            for j in range(n):
                # Using a helper that purely returns size
                def solve(r, c):
                    if r < 0 or c < 0: return 0
                    if (r, c) in memo: return memo[(r, c)]
                    if matrix[r][c] == '0':
                        memo[(r, c)] = 0
                        return 0

                    val = 1 + min(solve(r-1, c), solve(r, c-1), solve(r-1, c-1))
                    memo[(r, c)] = val
                    return val
                ans = max(ans, solve(i, j))
        return ans * ans
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if not matrix: return 0
        m, n = len(matrix), len(matrix[0])
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        max_side = 0

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if matrix[i - 1][j - 1] == '1':
                    dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
                    max_side = max(max_side, dp[i][j])

        return max_side * max_side
```

### Performance Optimization
**Space Compression:**
We can reduce the 2D DP table to a 1D array.

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if not matrix: return 0
        m, n = len(matrix), len(matrix[0])
        dp = [0] * (n + 1)
        max_side = 0
        prev = 0 # stores dp[i-1][j-1]

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                temp = dp[j]
                if matrix[i - 1][j - 1] == '1':
                    dp[j] = 1 + min(dp[j - 1], dp[j], prev)
                    max_side = max(max_side, dp[j])
                else:
                    dp[j] = 0
                prev = temp

        return max_side * max_side
```

### Complexity
- **Time:** $O(M \cdot N)$
- **Space:** $O(N)$ (optimized)
