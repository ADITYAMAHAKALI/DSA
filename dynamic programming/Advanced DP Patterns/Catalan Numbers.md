> [!note] Description
> Catalan numbers appear in many combinatorial problems involving recursive structures, such as parentheses, polygon triangulation, and binary trees.
> Formula: $C_n = \frac{1}{n+1}\binom{2n}{n} = \sum_{i=0}^{n-1} C_i C_{n-1-i}$.

## 1. Unique Binary Search Trees

### Intuition
Given an integer `n`, return the number of structurally unique BST's (binary search trees) which has exactly `n` nodes of unique values from `1` to `n`.
If we pick number `i` as the root ($1 \le i \le n$), then the left subtree will be formed by nodes $1 \dots i-1$ (count is $dp[i-1]$) and the right subtree by nodes $i+1 \dots n$ (count is $dp[n-i]$).
Total ways with `i` as root = $dp[i-1] \times dp[n-i]$.
Summing over all possible roots `i` gives the recurrence: $dp[n] = \sum_{i=1}^{n} dp[i-1] \times dp[n-i]$.
This is the defining recurrence for Catalan numbers.

### Top-Down (Memoization)
```python
class Solution:
    def numTrees(self, n: int) -> int:
        memo = {}

        def dp(num_nodes):
            if num_nodes <= 1:
                return 1
            if num_nodes in memo:
                return memo[num_nodes]

            total = 0
            for i in range(1, num_nodes + 1):
                # i is root
                left = i - 1
                right = num_nodes - i
                total += dp(left) * dp(right)

            memo[num_nodes] = total
            return total

        return dp(n)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 1

        for nodes in range(2, n + 1):
            for root in range(1, nodes + 1):
                left = root - 1
                right = nodes - root
                dp[nodes] += dp[left] * dp[right]

        return dp[n]
```

### Complexity
- **Time:** $O(N^2)$ (DP) or $O(N)$ (Math formula)
- **Space:** $O(N)$

---

## 2. Generate Parentheses (Counting)

### Intuition
This problem asks to generate all combinations, but for the DP pattern, we often just need to **count** them (which gives the Catalan number).
Problem: Count valid parentheses sequences of length `2n`.
Logic: A valid sequence (non-empty) can be represented as `(A)B`, where `A` and `B` are valid sequences.
If `A` has `k` pairs, `B` must have `n - 1 - k` pairs.
$C_n = \sum_{k=0}^{n-1} C_k C_{n-1-k}$.

### Top-Down (Memoization - for counting)
```python
class Solution:
    def countParenthesis(self, n: int) -> int:
        memo = {}
        def dp(k):
            if k == 0: return 1
            if k in memo: return memo[k]

            ans = 0
            for i in range(k):
                ans += dp(i) * dp(k - 1 - i)
            memo[k] = ans
            return ans
        return dp(n)
```

### Bottom-Up (Tabulation)
Same as Unique BSTs.

### Complexity
- **Time:** $O(N^2)$
- **Space:** $O(N)$

---

## 3. Minimum Score Triangulation of Polygon

### Intuition
Given `n` vertices of a convex polygon, you want to triangulate it into `n-2` triangles. The score is the sum of the products of the vertices of each triangle. Find min score.
Fix two adjacent vertices $i$ and $j$ (initially $0$ and $n-1$). To form a triangle with edge $ij$, we must pick a vertex $k$ ($i < k < j$).
The polygon splits into sub-polygons $(i, k)$ and $(k, j)$.
$dp[i][j] = \min_{k} (dp[i][k] + dp[k][j] + A[i]A[j]A[k])$.
This is structurally similar to MCM (Interval DP), but the *counting* version of triangulations is Catalan. The *optimization* version is this DP.

### Top-Down (Memoization)
```python
class Solution:
    def minScoreTriangulation(self, values: List[int]) -> int:
        n = len(values)
        memo = {}

        def dp(i, j):
            if j - i < 2:
                return 0
            if (i, j) in memo:
                return memo[(i, j)]

            res = float('inf')
            for k in range(i + 1, j):
                res = min(res, values[i] * values[k] * values[j] + dp(i, k) + dp(k, j))

            memo[(i, j)] = res
            return res

        return dp(0, n - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def minScoreTriangulation(self, values: List[int]) -> int:
        n = len(values)
        dp = [[0] * n for _ in range(n)]

        for length in range(2, n):
            for i in range(n - length):
                j = i + length
                dp[i][j] = float('inf')
                for k in range(i + 1, j):
                    dp[i][j] = min(dp[i][j], values[i]*values[k]*values[j] + dp[i][k] + dp[k][j])

        return dp[0][n - 1]
```

### Complexity
- **Time:** $O(N^3)$
- **Space:** $O(N^2)$
