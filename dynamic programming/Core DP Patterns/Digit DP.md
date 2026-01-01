> [!note] Description
> Digit DP is used to count integers in a range $[0, N]$ that satisfy certain properties based on their digits. The state usually includes `index`, `tight` constraint, and other properties.

## 1. Number of Digit One

### Intuition
Given an integer `n`, count the total number of digit 1 appearing in all non-negative integers less than or equal to `n`.
We can process digits from left to right.
State: `dp(index, count, is_tight, is_leading_zero)`
- `index`: current digit position being placed.
- `count`: number of 1s placed so far (not needed if we return sum, but helpful).
- `is_tight`: true if we are restricted by the digits of `n`.
- `is_leading_zero`: true if we are currently placing leading zeros.

### Top-Down (Memoization)
```python
class Solution:
    def countDigitOne(self, n: int) -> int:
        s = str(n)
        memo = {}

        def dp(index, count, is_tight):
            if index == len(s):
                return count
            state = (index, count, is_tight)
            if state in memo:
                return memo[state]

            upper_bound = int(s[index]) if is_tight else 9
            ans = 0

            for digit in range(upper_bound + 1):
                new_tight = is_tight and (digit == upper_bound)
                new_count = count + (1 if digit == 1 else 0)
                ans += dp(index + 1, new_count, new_tight)

            memo[state] = ans
            return ans

        return dp(0, 0, True)
```
*Note: The above approach is $O(10 \cdot \text{len} \cdot \text{len})$, actually counting every number. A better DP state would be to return (count of valid numbers, sum of property).*
Alternatively, `dp(index, is_tight)` returns the count of 1s in the suffixes.

### Improved Top-Down
```python
class Solution:
    def countDigitOne(self, n: int) -> int:
        s = str(n)
        memo = {}

        def dp(i, tight, count_ones):
            if i == len(s):
                return count_ones
            state = (i, tight, count_ones)
            if state in memo: return memo[state]

            limit = int(s[i]) if tight else 9
            res = 0
            for d in range(limit + 1):
                new_tight = tight and (d == limit)
                res += dp(i + 1, new_tight, count_ones + (1 if d == 1 else 0))

            memo[state] = res
            return res

        return dp(0, True, 0)
```
Wait, the state space for `count_ones` is up to 10. This works.

### Complexity
- **Time:** $O(\log N \cdot 10 \cdot \log N)$ (since max count is length of N)
- **Space:** $O(\log^2 N)$

---

## 2. Numbers At Most N Given Digit Set

### Intuition
Given an array of `digits` (as strings) and an integer `n`, return the number of positive integers that can be generated that are less than or equal to `n`.
Since the number of digits in `n` is small ($\le 9$), we can iterate.
Cases:
1. Numbers with fewer digits than `n`: these are always valid. If `n` has $L$ digits and `D` is size of set, then $\sum_{i=1}^{L-1} D^i$.
2. Numbers with same number of digits: Use Digit DP or simple iteration.

### Top-Down (Memoization)
```python
class Solution:
    def atMostNGivenDigitSet(self, digits: List[str], n: int) -> int:
        s = str(n)
        k = len(s)
        D = len(digits)

        # 1. Count numbers with fewer digits
        ans = sum(D ** i for i in range(1, k))

        memo = {}
        # 2. Count numbers with same length
        def dp(index, is_tight):
            if index == k:
                return 1
            state = (index, is_tight)
            if state in memo: return memo[state]

            limit = int(s[index]) if is_tight else 9
            res = 0
            for d_str in digits:
                d = int(d_str)
                if d < limit:
                    # If we pick a smaller digit, all subsequent positions can be anything
                    res += D ** (k - 1 - index)
                elif d == limit:
                    if is_tight:
                        res += dp(index + 1, True)
                    else:
                        res += D ** (k - 1 - index)
                else:
                    # d > limit, cannot place if tight
                    pass

            memo[state] = res
            return res

        return ans + dp(0, True)
```

### Complexity
- **Time:** $O(\log N \cdot |D|)$
- **Space:** $O(\log N)$

---

## 3. Counting Numbers with Unique Digits

### Intuition
Given an integer `n`, return the count of all numbers with unique digits, `x`, where $0 \le x < 10^n$.
For $n=0$, ans=1.
For $n=1$, ans=10.
For $n=2$, first digit 1-9 (9 choices), second 0-9 except first (9 choices) -> $9 \times 9 + 10$ (from n=1).
Formula: $9 \times 9 \times 8 \times \dots$

### Top-Down (Memoization / Combinatorics)
```python
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if n == 0: return 1

        memo = {}
        def dp(index, mask, is_leading_zero):
            if index == n:
                return 1
            state = (index, mask, is_leading_zero)
            if state in memo: return memo[state]

            res = 0
            for d in range(10):
                if mask & (1 << d): continue # Digit used

                new_mask = mask
                # If currently leading zero and we pick 0, it's still leading zero (mask doesn't change essentially for uniqueness check of 0, usually we handle 0 special)
                # Actually simpler: standard digit dp

                if is_leading_zero and d == 0:
                     res += dp(index + 1, 0, True)
                else:
                     res += dp(index + 1, mask | (1 << d), False)

            memo[state] = res
            return res

        # This standard DP counts numbers with exactly n digits (padded with 0).
        # But problem asks < 10^n.
        # Easier: Sum of k-digit numbers with unique digits for k in 1..n

        ans = 1 # for 0
        for k in range(1, n + 1):
            count = 9
            for i in range(k - 1):
                count *= (9 - i)
            ans += count
        return ans
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if n == 0: return 1

        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 10

        for i in range(2, n + 1):
            unique_len_i = 9
            for k in range(i - 1):
                unique_len_i *= (9 - k)
            dp[i] = dp[i - 1] + unique_len_i

        return dp[n]
```

### Complexity
- **Time:** $O(N)$
- **Space:** $O(N)$
