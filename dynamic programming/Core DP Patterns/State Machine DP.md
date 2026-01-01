> [!note] Description
> State Machine DP models problems where the entity transitions between finite states (e.g., Holding Stock, Sold Stock, Cooldown).

## 1. Best Time to Buy and Sell Stock with Cooldown

### Intuition
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day. You can buy and sell multiple times, but with a cooldown of 1 day after selling.
States for each day:
1.  **Held:** You have a stock (bought today or kept from before).
2.  **Sold:** You just sold a stock today.
3.  **Reset (Cooldown/Nothing):** You don't have a stock and didn't sell today (can buy tomorrow).

Transitions:
- Held: from `Held` (keep) or `Reset` (buy).
- Sold: from `Held` (sell).
- Reset: from `Reset` (wait) or `Sold` (cooldown from yesterday).

### Top-Down (Memoization)
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        memo = {}

        def dp(i, state):
            # state: 0=reset, 1=held, 2=sold
            if i >= n:
                return 0
            if (i, state) in memo:
                return memo[(i, state)]

            if state == 0: # Reset
                # Can buy or wait
                ans = max(-prices[i] + dp(i + 1, 1), dp(i + 1, 0))
            elif state == 1: # Held
                # Can sell or wait
                ans = max(prices[i] + dp(i + 1, 2), dp(i + 1, 1))
            else: # Sold
                # Must wait (cooldown)
                ans = dp(i + 1, 0)

            memo[(i, state)] = ans
            return ans

        return dp(0, 0)
```

### Bottom-Up (Tabulation - State Machine)
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0

        # held[i]: max profit on day i ending in Held state
        # sold[i]: max profit on day i ending in Sold state
        # reset[i]: max profit on day i ending in Reset state

        held = float('-inf')
        sold = float('-inf')
        reset = 0

        for price in prices:
            prev_held = held
            prev_sold = sold

            held = max(prev_held, reset - price)
            sold = prev_held + price
            reset = max(reset, prev_sold)

        return max(sold, reset)
```

### Complexity
- **Time:** $O(N)$
- **Space:** $O(1)$

---

## 2. Best Time to Buy and Sell Stock IV

### Intuition
You may complete at most `k` transactions.
States: `(day, transactions_left, holding)`.
`holding`: 0 or 1.

### Top-Down (Memoization)
```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        n = len(prices)
        memo = {}

        def dp(i, trans_left, holding):
            if i == n or trans_left == 0:
                return 0
            if (i, trans_left, holding) in memo:
                return memo[(i, trans_left, holding)]

            res = 0
            if holding:
                # Sell or Keep
                res = max(prices[i] + dp(i + 1, trans_left - 1, 0), dp(i + 1, trans_left, 1))
            else:
                # Buy or Wait
                res = max(-prices[i] + dp(i + 1, trans_left, 1), dp(i + 1, trans_left, 0))

            memo[(i, trans_left, holding)] = res
            return res

        return dp(0, k, 0)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if not prices: return 0

        # Optimization: If k is huge, it's unlimited transactions
        if k >= len(prices) // 2:
            return sum(max(prices[i+1] - prices[i], 0) for i in range(len(prices)-1))

        # dp[i][j][0] = max profit on day i, j transactions left, not holding
        # Optimized to current/prev arrays or 1D

        # buy[j] = max profit with j transactions left, holding stock
        # sell[j] = max profit with j transactions left, sold stock

        buy = [float('-inf')] * (k + 1)
        sell = [0] * (k + 1)

        for price in prices:
            for j in range(1, k + 1):
                buy[j] = max(buy[j], sell[j-1] - price)
                sell[j] = max(sell[j], buy[j] + price)

        return sell[k]
```

### Complexity
- **Time:** $O(N \cdot K)$
- **Space:** $O(K)$

---

## 3. Student Attendance Record II

### Intuition
Record of length `n` with 'A' (Absent), 'L' (Late), 'P' (Present).
Valid if:
1. Total 'A' < 2.
2. No 3 consecutive 'L'.
State: `(index, count_A, count_consecutive_L)`.

### Top-Down (Memoization)
```python
class Solution:
    def checkRecord(self, n: int) -> int:
        MOD = 10**9 + 7
        memo = {}

        def dp(i, a_count, l_count):
            if i == n:
                return 1
            if (i, a_count, l_count) in memo:
                return memo[(i, a_count, l_count)]

            res = 0
            # 1. Add 'P'
            res = (res + dp(i + 1, a_count, 0)) % MOD

            # 2. Add 'A'
            if a_count == 0:
                res = (res + dp(i + 1, 1, 0)) % MOD

            # 3. Add 'L'
            if l_count < 2:
                res = (res + dp(i + 1, a_count, l_count + 1)) % MOD

            memo[(i, a_count, l_count)] = res
            return res

        return dp(0, 0, 0)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def checkRecord(self, n: int) -> int:
        MOD = 10**9 + 7
        # dp[i][j][k] -> length i, j 'A's, k ending 'L's
        dp = [[0] * 3 for _ in range(2)]
        dp[0][0] = 1

        for i in range(n):
            new_dp = [[0] * 3 for _ in range(2)]

            # For each state
            for a in range(2):
                for l in range(3):
                    val = dp[a][l]
                    if val == 0: continue

                    # Add 'P' -> reset l to 0
                    new_dp[a][0] = (new_dp[a][0] + val) % MOD

                    # Add 'A' -> inc a, reset l to 0
                    if a == 0:
                        new_dp[1][0] = (new_dp[1][0] + val) % MOD

                    # Add 'L' -> inc l
                    if l < 2:
                        new_dp[a][l+1] = (new_dp[a][l+1] + val) % MOD
            dp = new_dp

        return sum(sum(row) for row in dp) % MOD
```

### Complexity
- **Time:** $O(N)$
- **Space:** $O(1)$ (using fixed size DP table)
