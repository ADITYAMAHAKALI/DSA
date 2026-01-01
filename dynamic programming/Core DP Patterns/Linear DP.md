> [!note] Description
> Linear DP involves problems where the state at index `i` depends only on a fixed number of previous states (e.g., `i-1`, `i-2`). The recurrence relation is usually linear.

## 1. Climbing Stairs

### Intuition
You are climbing a staircase. It takes `n` steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

The intuition is that to reach step `i`, you must have come from either step `i-1` (taking 1 step) or step `i-2` (taking 2 steps). Thus, the number of ways to reach step `i` is the sum of ways to reach `i-1` and `i-2`. This is exactly the Fibonacci sequence.

### Top-Down (Memoization)
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        memo = {}

        def dp(i):
            if i <= 2:
                return i
            if i in memo:
                return memo[i]

            memo[i] = dp(i - 1) + dp(i - 2)
            return memo[i]

        return dp(n)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        dp = [0] * (n + 1)
        dp[1] = 1
        dp[2] = 2

        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]

        return dp[n]
```

### Performance Optimization
**State Compression:**
Since we only need the last two values (`dp[i-1]` and `dp[i-2]`) to calculate the current value, we don't need an array of size `n`. We can use two variables.

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        prev2, prev1 = 1, 2

        for i in range(3, n + 1):
            current = prev1 + prev2
            prev2 = prev1
            prev1 = current

        return prev1
```

### Complexity
- **Time:** $O(n)$
- **Space:** $O(1)$ (optimized), $O(n)$ (basic DP)

---

## 2. House Robber

### Intuition
You are a robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

For each house `i`, you have two choices:
1.  **Rob it:** Then you cannot rob house `i-1`, so you add the money from house `i` to the max money from house `i-2`.
2.  **Skip it:** Then you take the max money from house `i-1`.

### Top-Down (Memoization)
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        memo = {}

        def dp(i):
            if i < 0:
                return 0
            if i in memo:
                return memo[i]

            # Recurrence: max(rob current + prev-2, skip current + prev-1)
            memo[i] = max(nums[i] + dp(i - 2), dp(i - 1))
            return memo[i]

        return dp(n - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0
        n = len(nums)
        if n == 1:
            return nums[0]

        dp = [0] * n
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])

        for i in range(2, n):
            dp[i] = max(nums[i] + dp[i - 2], dp[i - 1])

        return dp[n - 1]
```

### Performance Optimization
**State Compression:**
Similar to climbing stairs, we only need the previous two states.

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums: return 0

        prev2 = 0
        prev1 = 0

        for num in nums:
            current = max(prev2 + num, prev1)
            prev2 = prev1
            prev1 = current

        return prev1
```

### Complexity
- **Time:** $O(n)$
- **Space:** $O(1)$ (optimized)

---

## 3. Maximum Subarray

### Intuition
Given an integer array `nums`, find the subarray with the largest sum, and return its sum.

This is famously known as **Kadane's Algorithm**. The intuition is: for each element `nums[i]`, do we start a new subarray here, or do we extend the existing subarray ending at `nums[i-1]`?
If the sum of the subarray ending at `nums[i-1]` is negative, adding it to `nums[i]` will only make `nums[i]` smaller. So, we should start fresh at `nums[i]`. Otherwise, we extend.

### Top-Down (Memoization)
Although typically solved iteratively, we can frame it recursively: `dp[i]` is the max subarray sum ending at index `i`.
`dp[i] = max(nums[i], nums[i] + dp[i-1])`

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        n = len(nums)
        memo = {}

        def get_max_ending_at(i):
            if i == 0:
                return nums[0]
            if i in memo:
                return memo[i]

            # Either extend the previous subarray or start new
            memo[i] = max(nums[i], nums[i] + get_max_ending_at(i - 1))
            return memo[i]

        # We need the max of all dp states, not just the last one
        max_sum = -float('inf')
        for i in range(n):
            max_sum = max(max_sum, get_max_ending_at(i))

        return max_sum
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [0] * n
        dp[0] = nums[0]
        max_so_far = nums[0]

        for i in range(1, n):
            dp[i] = max(nums[i], nums[i] + dp[i - 1])
            max_so_far = max(max_so_far, dp[i])

        return max_so_far
```

### Performance Optimization
**State Compression (Kadane's Algorithm):**
We only need the previous sum to calculate the current sum.

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_so_far = nums[0]
        current_sum = nums[0]

        for i in range(1, len(nums)):
            # If current_sum is negative, discard it (start fresh at nums[i])
            # effectively: current_sum = max(nums[i], current_sum + nums[i])
            if current_sum < 0:
                current_sum = nums[i]
            else:
                current_sum += nums[i]

            max_so_far = max(max_so_far, current_sum)

        return max_so_far
```

### Complexity
- **Time:** $O(n)$
- **Space:** $O(1)$
