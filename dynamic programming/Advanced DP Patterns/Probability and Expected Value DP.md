> [!note] Description
> Probability DP solves problems where transitions depend on random events. The value of a state is usually the expected value or sum of probabilities of next states.

## 1. Knight Probability in Chessboard

### Intuition
A knight starts at `(row, col)` on an `n x n` board. It makes `k` moves. At each step, it chooses one of 8 moves uniformly at random. Find the probability that the knight remains on the board.
`dp[k][r][c]` = probability knight is at `(r, c)` after `k` moves.
`dp[k][r][c] = sum(dp[k-1][prev_r][prev_c] / 8)` for all valid previous positions.
Or forward: `dp[k+1][next_r][next_c] += dp[k][r][c] / 8`.

### Top-Down (Memoization)
```python
class Solution:
    def knightProbability(self, n: int, k: int, row: int, column: int) -> float:
        memo = {}
        moves = [(1, 2), (1, -2), (-1, 2), (-1, -2),
                 (2, 1), (2, -1), (-2, 1), (-2, -1)]

        def dp(step, r, c):
            if not (0 <= r < n and 0 <= c < n):
                return 0
            if step == 0:
                return 1
            if (step, r, c) in memo:
                return memo[(step, r, c)]

            prob = 0
            for dr, dc in moves:
                prob += 0.125 * dp(step - 1, r + dr, c + dc)

            memo[(step, r, c)] = prob
            return prob

        return dp(k, row, column)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def knightProbability(self, n: int, k: int, row: int, column: int) -> float:
        dp = [[0] * n for _ in range(n)]
        dp[row][column] = 1

        moves = [(1, 2), (1, -2), (-1, 2), (-1, -2),
                 (2, 1), (2, -1), (-2, 1), (-2, -1)]

        for step in range(k):
            new_dp = [[0] * n for _ in range(n)]
            for r in range(n):
                for c in range(n):
                    if dp[r][c] > 0:
                        for dr, dc in moves:
                            nr, nc = r + dr, c + dc
                            if 0 <= nr < n and 0 <= nc < n:
                                new_dp[nr][nc] += dp[r][c] / 8.0
            dp = new_dp

        return sum(sum(row) for row in dp)
```

### Complexity
- **Time:** $O(K \cdot N^2)$
- **Space:** $O(N^2)$

---

## 2. New 21 Game

### Intuition
Alice plays a game with cards [1..W]. She starts with 0 and draws numbers while sum < K. What is the probability that sum <= N?
`dp[i]` = probability that the current sum is `i`.
`dp[i] = (dp[i-1] + ... + dp[i-W]) / W`.
This is a sliding window sum.

### Bottom-Up (Tabulation)
```python
class Solution:
    def new21Game(self, n: int, k: int, maxPts: int) -> float:
        if k == 0 or n >= k + maxPts:
            return 1.0

        dp = [0.0] * (n + 1)
        dp[0] = 1.0

        window_sum = 1.0
        ans = 0.0

        for i in range(1, n + 1):
            dp[i] = window_sum / maxPts

            if i < k:
                window_sum += dp[i]
            else:
                ans += dp[i] # If we reach >= k, we stop.

            # Maintain window of size maxPts (remove i - maxPts)
            if i >= maxPts:
                window_sum -= dp[i - maxPts]

        return ans
```

### Complexity
- **Time:** $O(N)$
- **Space:** $O(N)$

---

## 3. Soup Servings

### Intuition
Soup A and Soup B, initially `N` ml each. Four operations with prob 0.25:
(100, 0), (75, 25), (50, 50), (25, 75).
Stop when A or B is empty. Return prob that A becomes empty first + 0.5 * prob that both empty same time.
If `N` is large, probability approaches 1 (A empties faster on average).

### Top-Down (Memoization)
```python
class Solution:
    def soupServings(self, n: int) -> float:
        if n > 4800: return 1.0 # Optimization for large N

        memo = {}
        def dp(a, b):
            if a <= 0 and b <= 0: return 0.5
            if a <= 0: return 1.0
            if b <= 0: return 0.0

            if (a, b) in memo: return memo[(a, b)]

            res = 0.25 * (dp(a - 100, b) +
                          dp(a - 75, b - 25) +
                          dp(a - 50, b - 50) +
                          dp(a - 25, b - 75))

            memo[(a, b)] = res
            return res

        return dp(n, n)
```

### Complexity
- **Time:** $O(1)$ (due to threshold cap 4800)
- **Space:** $O(1)$
