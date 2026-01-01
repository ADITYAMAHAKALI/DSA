> [!note] Description
> Insertion DP involves maintaining a state while iteratively inserting elements into a structure (often a permutation), used for counting permutations with specific properties.

## 1. K Inverse Pairs Array

### Intuition
Given `n` and `k`, find the number of arrays of length `n` consisting of numbers `1` to `n` such that there are exactly `k` inverse pairs.
When we insert the number `n` into a permutation of `1..n-1`, if we place it at position `i` (0-indexed from right), we create `i` new inverse pairs (since `n` is larger than all existing numbers).
`dp[i][j]` = count of permutations of length `i` with `j` inverse pairs.
`dp[i][j] = sum(dp[i-1][j - p])` for `0 <= p < i`.

### Top-Down (Memoization)
```python
class Solution:
    def kInversePairs(self, n: int, k: int) -> int:
        MOD = 10**9 + 7
        memo = {}

        def dp(i, j):
            if j == 0: return 1
            if j < 0: return 0
            if i == 0: return 0

            if (i, j) in memo: return memo[(i, j)]

            # Naive recursion is O(N*K*N), too slow.
            # We need the optimized recurrence from Bottom-Up logic essentially.
            # But straightforward top-down:
            val = 0
            for p in range(min(j, i - 1) + 1):
                val = (val + dp(i - 1, j - p)) % MOD

            memo[(i, j)] = val
            return val

        # Optimization: dp[i][j] = dp[i][j-1] + dp[i-1][j] - dp[i-1][j-i]
        # This sliding window optimization is easier in Tabulation.
        return self.solveTabulation(n, k)

    def solveTabulation(self, n, k):
        MOD = 10**9 + 7
        dp = [[0] * (k + 1) for _ in range(n + 1)]
        dp[0][0] = 1

        for i in range(1, n + 1):
            dp[i][0] = 1
            for j in range(1, k + 1):
                val = (dp[i][j-1] + dp[i-1][j]) % MOD
                if j >= i:
                    val = (val - dp[i-1][j-i] + MOD) % MOD
                dp[i][j] = val

        return dp[n][k]
```

### Complexity
- **Time:** $O(N \cdot K)$
- **Space:** $O(K)$

---

## 2. Valid Permutations for DI Sequence

### Intuition
Given a string `s` of length `n` containing 'D' (decreasing) and 'I' (increasing). Count permutations of `0..n` satisfying the relations.
When placing the number `i` (ranging from 0 to n), we only care about its relative rank among the numbers currently available or previously placed.
Standard approach: Insert numbers `0` to `n` one by one?
Or better: `dp[i][j]` = number of valid permutations of length `i+1` ending with rank `j` (meaning it is the $(j+1)$-th smallest number among those used).

### Bottom-Up (Tabulation)
```python
class Solution:
    def numPermsDISequence(self, s: str) -> int:
        n = len(s)
        MOD = 10**9 + 7
        # dp[i][j]: permutations of first i+1 numbers (0..i) ending with rank j
        dp = [[0] * (n + 1) for _ in range(n + 1)]
        dp[0][0] = 1

        for i in range(n):
            if s[i] == 'I':
                # Previous rank k must be < current rank j
                # Current rank j means j-th smallest.
                # If we pick j-th smallest, previous numbers >= j get shifted up.
                # Essentially we sum dp[i][k] for k < j
                cur_sum = 0
                for j in range(i + 2):
                    dp[i+1][j] = cur_sum
                    if j <= i:
                        cur_sum = (cur_sum + dp[i][j]) % MOD
            else:
                # Previous rank k must be >= current rank j
                cur_sum = 0
                for j in range(i + 1, -1, -1):
                    cur_sum = (cur_sum + dp[i][j]) % MOD
                    dp[i+1][j] = cur_sum

        return sum(dp[n]) % MOD
```

### Complexity
- **Time:** $O(N^2)$
- **Space:** $O(N^2)$

---

## 3. Permutation Sequence

### Intuition
The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations. By listing and labeling all of the permutations in order, find the `k`-th permutation.
This is not strictly DP but a combinatorial construction (often called "Digit DP style" or "Insertion Logic").
We determine digits from left to right.
For the first position, if we pick `1`, there are `(n-1)!` permutations. If `k > (n-1)!`, it's not starting with 1. We skip blocks of `(n-1)!` size.

### Implementation
```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        numbers = [str(i) for i in range(1, n + 1)]
        fact = [1] * n
        for i in range(1, n):
            fact[i] = i * fact[i - 1]

        k -= 1 # 0-indexed
        ans = []

        for i in range(n, 0, -1):
            # Size of block for current position
            block_size = fact[i - 1]
            index = k // block_size
            k %= block_size

            ans.append(numbers[index])
            numbers.pop(index)

        return "".join(ans)
```

### Complexity
- **Time:** $O(N^2)$ (due to pop from list)
- **Space:** $O(N)$
