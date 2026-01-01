> [!note] Description
> LCS and String DP problems involve comparing two sequences to find commonalities, differences, or transformation costs. The state typically involves indices `i` and `j` representing prefixes of the two strings.

## 1. Longest Common Subsequence

### Intuition
Given two strings `text1` and `text2`, return the length of their longest common subsequence.
If characters `text1[i]` and `text2[j]` match, they contribute 1 to the result, and we look at `i-1, j-1`. If they don't match, we must either skip a character from `text1` or from `text2`, taking the maximum of those two options.

### Top-Down (Memoization)
```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        memo = {}

        def dp(i, j):
            if i < 0 or j < 0:
                return 0

            state = (i, j)
            if state in memo:
                return memo[state]

            if text1[i] == text2[j]:
                memo[state] = 1 + dp(i - 1, j - 1)
            else:
                memo[state] = max(dp(i - 1, j), dp(i, j - 1))

            return memo[state]

        return dp(m - 1, n - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        # dp[i][j] represents LCS of text1[0..i-1] and text2[0..j-1]
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    dp[i][j] = 1 + dp[i - 1][j - 1]
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

        return dp[m][n]
```

### Performance Optimization
**State Compression:**
We only need the previous row to calculate the current row.

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        if m < n: # Ensure smaller string is used for space optimization
            return self.longestCommonSubsequence(text2, text1)

        prev = [0] * (n + 1)
        curr = [0] * (n + 1)

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    curr[j] = 1 + prev[j - 1]
                else:
                    curr[j] = max(prev[j], curr[j - 1])
            prev = list(curr)

        return prev[n]
```

### Complexity
- **Time:** $O(M \cdot N)$
- **Space:** $O(\min(M, N))$ (optimized)

---

## 2. Edit Distance

### Intuition
Given two strings `word1` and `word2`, return the minimum number of operations (insert, delete, replace) to convert `word1` to `word2`.
For `word1[i]` and `word2[j]`:
- If match: no op needed, move to `i-1, j-1`.
- If mismatch:
    - Insert: move to `i, j-1`
    - Delete: move to `i-1, j`
    - Replace: move to `i-1, j-1`
    Take the minimum of these + 1.

### Top-Down (Memoization)
```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        memo = {}

        def dp(i, j):
            if i < 0: return j + 1 # Need to insert remaining j+1 chars
            if j < 0: return i + 1 # Need to delete remaining i+1 chars

            if (i, j) in memo: return memo[(i, j)]

            if word1[i] == word2[j]:
                res = dp(i - 1, j - 1)
            else:
                res = 1 + min(
                    dp(i, j - 1),    # Insert
                    dp(i - 1, j),    # Delete
                    dp(i - 1, j - 1) # Replace
                )
            memo[(i, j)] = res
            return res

        return dp(m - 1, n - 1)
```

### Bottom-Up (Tabulation)
```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        # Base cases
        for i in range(m + 1): dp[i][0] = i
        for j in range(n + 1): dp[0][j] = j

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = 1 + min(dp[i - 1][j],    # Delete
                                       dp[i][j - 1],    # Insert
                                       dp[i - 1][j - 1])# Replace

        return dp[m][n]
```

### Performance Optimization
**State Compression:**
Similar to LCS, we can reduce space to two rows (or even one row with careful variable management).

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        prev = [j for j in range(n + 1)]
        curr = [0] * (n + 1)

        for i in range(1, m + 1):
            curr[0] = i
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    curr[j] = prev[j - 1]
                else:
                    curr[j] = 1 + min(prev[j], curr[j - 1], prev[j - 1])
            prev = list(curr)

        return prev[n]
```

### Complexity
- **Time:** $O(M \cdot N)$
- **Space:** $O(N)$ (optimized)

---

## 3. Shortest Common Supersequence

### Intuition
Given strings `str1` and `str2`, return the shortest string that has both `str1` and `str2` as subsequences.
The length of the SCS is `len(str1) + len(str2) - len(LCS)`. To reconstruct the string, we first compute the LCS DP table, then backtrack from `dp[m][n]` to `dp[0][0]`. If characters match, add them once. If not, add the character corresponding to the direction of the "max" (or in this case, we effectively merge them).

### Top-Down (Memoization)
The logic focuses on finding LCS first, then reconstruction. The length calculation is trivial if LCS is known.
Code below implements reconstruction using the Tabulation table of LCS.

### Bottom-Up (Tabulation & Reconstruction)
```python
class Solution:
    def shortestCommonSupersequence(self, str1: str, str2: str) -> str:
        m, n = len(str1), len(str2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        # Build LCS table
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if str1[i - 1] == str2[j - 1]:
                    dp[i][j] = 1 + dp[i - 1][j - 1]
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

        # Backtrack to build SCS
        i, j = m, n
        res = []
        while i > 0 and j > 0:
            if str1[i - 1] == str2[j - 1]:
                res.append(str1[i - 1])
                i -= 1
                j -= 1
            elif dp[i - 1][j] > dp[i][j - 1]:
                res.append(str1[i - 1])
                i -= 1
            else:
                res.append(str2[j - 1])
                j -= 1

        # Append remaining characters
        while i > 0:
            res.append(str1[i - 1])
            i -= 1
        while j > 0:
            res.append(str2[j - 1])
            j -= 1

        return "".join(res[::-1])
```

### Performance Optimization
Generally, $O(M \cdot N)$ time is required to build the table for reconstruction. Space is $O(M \cdot N)$ because we need the full table to backtrack path. Bitset optimization can improve constant factors but complexity remains same.

### Complexity
- **Time:** $O(M \cdot N)$
- **Space:** $O(M \cdot N)$
