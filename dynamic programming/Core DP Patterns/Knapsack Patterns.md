> [!note] Description
> Knapsack problems involve selecting items with given weights and values to maximize total value without exceeding a capacity constraint, or finding if a subset sum exists.

## 1. Partition Equal Subset Sum

### Intuition
Given an array `nums`, determine if it can be partitioned into two subsets with equal sums.
This is equivalent to finding if there exists a subset with sum equal to `TotalSum / 2`. If `TotalSum` is odd, it's impossible.
This is a variation of the **0/1 Knapsack** problem where "weight" is the number value and we want to exactly fill a capacity of `TotalSum / 2`.

### Top-Down (Memoization)
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total_sum = sum(nums)
        if total_sum % 2 != 0:
            return False

        target = total_sum // 2
        memo = {}

        def dp(index, current_sum):
            if current_sum == target:
                return True
            if current_sum > target or index >= len(nums):
                return False

            state = (index, current_sum)
            if state in memo:
                return memo[state]

            # Choice: Include nums[index] or Exclude nums[index]
            memo[state] = dp(index + 1, current_sum + nums[index]) or dp(index + 1, current_sum)
            return memo[state]

        return dp(0, 0)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total_sum = sum(nums)
        if total_sum % 2 != 0:
            return False
        target = total_sum // 2
        n = len(nums)

        # dp[i][j] means if sum 'j' is possible using first 'i' items
        dp = [[False] * (target + 1) for _ in range(n + 1)]

        # Base case: Sum 0 is always possible (empty set)
        for i in range(n + 1):
            dp[i][0] = True

        for i in range(1, n + 1):
            for j in range(1, target + 1):
                if nums[i-1] <= j:
                    # Include or Exclude
                    dp[i][j] = dp[i-1][j - nums[i-1]] or dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j]

        return dp[n][target]
```

### Performance Optimization
**State Compression:**
We only need the previous row to calculate the current row. Even better, we can iterate backwards to use a 1D array.

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total_sum = sum(nums)
        if total_sum % 2 != 0:
            return False
        target = total_sum // 2

        dp = [False] * (target + 1)
        dp[0] = True

        for num in nums:
            # Iterate backwards to avoid using the same element multiple times for the same sum
            for j in range(target, num - 1, -1):
                dp[j] = dp[j] or dp[j - num]

        return dp[target]
```

### Complexity
- **Time:** $O(N \cdot \text{Target})$ where $N$ is array length and Target is `sum/2`.
- **Space:** $O(\text{Target})$ (optimized).

---

## 2. Coin Change

### Intuition
You are given an integer array `coins` representing coins of different denominations and an integer `amount`. Return the fewest number of coins that you need to make up that amount.
This is an **Unbounded Knapsack** problem because we can use each coin infinite times. We want to minimize the number of items to reach a target "weight".

### Top-Down (Memoization)
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        memo = {}

        def dp(rem):
            if rem == 0:
                return 0
            if rem < 0:
                return float('inf')
            if rem in memo:
                return memo[rem]

            min_coins = float('inf')
            for coin in coins:
                res = dp(rem - coin)
                if res != float('inf'):
                    min_coins = min(min_coins, res + 1)

            memo[rem] = min_coins
            return min_coins

        result = dp(amount)
        return result if result != float('inf') else -1
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        # Initialize dp array with value > amount (as infinity)
        dp = [float('inf')] * (amount + 1)
        dp[0] = 0

        for i in range(1, amount + 1):
            for coin in coins:
                if i - coin >= 0:
                    dp[i] = min(dp[i], dp[i - coin] + 1)

        return dp[amount] if dp[amount] != float('inf') else -1
```

### Performance Optimization
The iterative solution above is already space-optimized to $O(\text{Amount})$ (1D array) because for unbounded knapsack, `dp[i]` depends on `dp[i - coin]`, which is a value in the *current* row (or iteration space), unlike 0/1 knapsack where it depends on the *previous* row.

### Complexity
- **Time:** $O(\text{Amount} \cdot \text{Coins})$
- **Space:** $O(\text{Amount})$

---

## 3. Target Sum

### Intuition
You are given an integer array `nums` and an integer `target`. You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer and then concatenating all the integers. Return the number of different expressions that you can build which evaluate to `target`.

Let $P$ be the set of positive numbers and $N$ be the set of negative numbers.
Sum($P$) - Sum($N$) = Target
Sum($P$) + Sum($N$) + Sum($P$) - Sum($N$) = Target + Sum($P$) + Sum($N$)
2 * Sum($P$) = Target + TotalSum
Sum($P$) = (Target + TotalSum) / 2
So this problem reduces to: Find the number of subsets with sum equal to `(Target + TotalSum) / 2`. This is exactly the **0/1 Knapsack / Subset Sum** problem.

### Top-Down (Memoization)
```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        memo = {}

        def dp(index, current_sum):
            if index == len(nums):
                return 1 if current_sum == target else 0

            state = (index, current_sum)
            if state in memo:
                return memo[state]

            # Choice: Add or Subtract
            add = dp(index + 1, current_sum + nums[index])
            sub = dp(index + 1, current_sum - nums[index])

            memo[state] = add + sub
            return memo[state]

        return dp(0, 0)
```

### Bottom-Up (Tabulation - Optimized Subset Sum)
Using the transformation to Subset Sum `(Target + TotalSum) / 2`.

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        total_sum = sum(nums)

        # Check validity
        if abs(target) > total_sum or (total_sum + target) % 2 != 0:
            return 0

        subset_target = (total_sum + target) // 2

        dp = [0] * (subset_target + 1)
        dp[0] = 1

        for num in nums:
            for j in range(subset_target, num - 1, -1):
                dp[j] += dp[j - num]

        return dp[subset_target]
```

### Performance Optimization
**State Compression:**
The 1D array approach used in the Bottom-Up solution is the standard space optimization for 0/1 Knapsack problems.

### Complexity
- **Time:** $O(N \cdot \text{Sum})$
- **Space:** $O(\text{Sum})$
