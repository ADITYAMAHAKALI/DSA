> [!note] Description
> Interval DP problems involve finding an optimal solution for a range `[i, j]` by combining solutions from smaller sub-intervals. The state usually represents the range `(i, j)`.

## 1. Matrix Chain Multiplication

### Intuition
Given a sequence of matrices, find the most efficient way to multiply these matrices together. The cost of multiplying a matrix of size $p \times q$ with $q \times r$ is $p \cdot q \cdot r$.
We want to parenthesize the product $A_1 \dots A_n$ such that the total cost is minimized.
For a range $[i, j]$, we can split it at any $k$ ($i \le k < j$) into $[i, k]$ and $[k+1, j]$.
$Cost(i, j) = \min_{k} (Cost(i, k) + Cost(k+1, j) + \text{dims}[i-1] \cdot \text{dims}[k] \cdot \text{dims}[j])$

### Top-Down (Memoization)
```python
import sys

class Solution:
    def matrixMultiplication(self, N, arr):
        memo = {}

        def dp(i, j):
            if i == j:
                return 0
            if (i, j) in memo:
                return memo[(i, j)]

            min_cost = sys.maxsize
            for k in range(i, j):
                # arr[i-1] is row of matrix i, arr[k] is col of i (and row of k+1), arr[j] is col of j
                cost = dp(i, k) + dp(k + 1, j) + arr[i - 1] * arr[k] * arr[j]
                min_cost = min(min_cost, cost)

            memo[(i, j)] = min_cost
            return min_cost

        return dp(1, N - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def matrixMultiplication(self, N, arr):
        dp = [[0] * N for _ in range(N)]

        # Length of chain L
        for L in range(2, N):
            for i in range(1, N - L + 1):
                j = i + L - 1
                dp[i][j] = float('inf')
                for k in range(i, j):
                    q = dp[i][k] + dp[k + 1][j] + arr[i - 1] * arr[k] * arr[j]
                    if q < dp[i][j]:
                        dp[i][j] = q

        return dp[1][N - 1]
```

### Complexity
- **Time:** $O(N^3)$
- **Space:** $O(N^2)$

---

## 2. Burst Balloons

### Intuition
Given `n` balloons, indexed 0 to `n-1`. Each balloon is painted with a number on it represented by array `nums`. You are asked to burst all the balloons. If you burst balloon `i` you will get `nums[i-1] * nums[i] * nums[i+1]` coins. After the burst, `nums[i-1]` and `nums[i+1]` become adjacent.
Thinking backwards: Instead of picking which balloon to burst *first*, pick which one to burst *last*. If balloon `k` is the last one to burst in range `(i, j)`, then it separates the problem into subproblems `(i, k)` and `(k, j)`, and the cost is `nums[i] * nums[k] * nums[j]` (using the virtual boundaries).

### Top-Down (Memoization)
```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        # Pad with 1s
        nums = [1] + nums + [1]
        n = len(nums)
        memo = {}

        def dp(left, right):
            # No balloons between left and right
            if left + 1 == right:
                return 0
            if (left, right) in memo:
                return memo[(left, right)]

            res = 0
            # Try every balloon as the LAST one to burst
            for i in range(left + 1, right):
                current = nums[left] * nums[i] * nums[right]
                res = max(res, current + dp(left, i) + dp(i, right))

            memo[(left, right)] = res
            return res

        return dp(0, n - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + nums + [1]
        n = len(nums)
        dp = [[0] * n for _ in range(n)]

        for length in range(2, n): # Length of range
            for left in range(n - length):
                right = left + length
                for i in range(left + 1, right):
                    coins = nums[left] * nums[i] * nums[right]
                    coins += dp[left][i] + dp[i][right]
                    dp[left][right] = max(dp[left][right], coins)

        return dp[0][n - 1]
```

### Complexity
- **Time:** $O(N^3)$
- **Space:** $O(N^2)$

---

## 3. Palindrome Partitioning II

### Intuition
Given a string `s`, partition `s` such that every substring of the partition is a palindrome. Return the minimum cuts needed.
We can precompute a palindrome table `isPal[i][j]`. Then define `dp[i]` as the min cuts for the suffix `s[i:]` (or prefix).
`dp[i] = 1 + min(dp[j+1])` for all `j >= i` where `s[i..j]` is a palindrome.

### Top-Down (Memoization)
```python
class Solution:
    def minCut(self, s: str) -> int:
        n = len(s)
        memo_pal = {}
        memo_cut = {}

        def is_palindrome(i, j):
            if i >= j: return True
            if (i, j) in memo_pal: return memo_pal[(i, j)]
            if s[i] == s[j]:
                memo_pal[(i, j)] = is_palindrome(i + 1, j - 1)
            else:
                memo_pal[(i, j)] = False
            return memo_pal[(i, j)]

        def dp(i):
            if i == n:
                return -1 # offset the +1 in the recurrence
            if i in memo_cut:
                return memo_cut[i]

            min_cuts = float('inf')
            for j in range(i, n):
                if is_palindrome(i, j):
                    min_cuts = min(min_cuts, 1 + dp(j + 1))

            memo_cut[i] = min_cuts
            return min_cuts

        return dp(0)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def minCut(self, s: str) -> int:
        n = len(s)
        # is_pal[i][j] is True if s[i:j+1] is a palindrome
        is_pal = [[False] * n for _ in range(n)]

        for length in range(1, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1
                if length == 1:
                    is_pal[i][j] = True
                elif length == 2:
                    is_pal[i][j] = (s[i] == s[j])
                else:
                    is_pal[i][j] = (s[i] == s[j] and is_pal[i + 1][j - 1])

        # dp[i] = min cuts for s[0...i]
        dp = [0] * n
        for i in range(n):
            if is_pal[0][i]:
                dp[i] = 0
            else:
                min_val = float('inf')
                for j in range(i):
                    if is_pal[j + 1][i]:
                        min_val = min(min_val, dp[j] + 1)
                dp[i] = min_val

        return dp[n - 1]
```

### Performance Optimization
The palindrome check and the DP can be built simultaneously or optimized to $O(N^2)$ time.
Without precomputing `is_pal`, checking palindrome is $O(N)$, making total time $O(N^3)$. With precomputation, it is $O(N^2)$.

### Complexity
- **Time:** $O(N^2)$
- **Space:** $O(N^2)$
