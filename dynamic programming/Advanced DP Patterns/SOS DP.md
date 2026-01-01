> [!note] Description
> SOS DP (Sum Over Subsets) is a technique to compute the sum of values for all subsets of a mask in $O(N \cdot 2^N)$ instead of $O(3^N)$.
> Generally used when we need $F[mask] = \sum_{sub \subseteq mask} A[sub]$.

## 1. Sum Over Subsets (Basic Problem)

### Intuition
Given an array `A` of size $2^N$. We want to compute an array `F` where `F[mask]` is the sum of `A[i]` for all `i` such that `i & mask == i` (i is a submask of mask).
Brute force is $O(4^N)$ or $O(3^N)$. SOS DP does this by iterating through each bit position.
`dp[mask][i]` = sum of subsets of `mask` where the first `i` bits can differ, but remaining bits must match `mask`.

### Bottom-Up (Tabulation - Optimized)
```python
class Solution:
    def solveSOS(self, A: List[int], n: int) -> List[int]:
        # A has size 2^n
        dp = list(A) # Initialize with A values

        for i in range(n): # Iterate over each bit position
            for mask in range(1 << n):
                if mask & (1 << i):
                    # If bit i is set in mask, add the value where bit i is NOT set
                    dp[mask] += dp[mask ^ (1 << i)]

        return dp
```

### Complexity
- **Time:** $O(N \cdot 2^N)$
- **Space:** $O(2^N)$

---

## 2. Maximum Compatibility Score Sum

### Intuition
There are `m` students and `m` mentors. You are given two matrices. Score of a pair is the number of matching answers. Find optimal pairing to maximize total score. `m <= 8`.
This can be solved with Bitmask DP (Assignment problem).
State: `dp[mask]` = max score using a subset of mentors represented by `mask`, assigning them to the first `k` students (where `k` is number of set bits).

### Top-Down (Memoization)
```python
class Solution:
    def maxCompatibilitySum(self, students: List[List[int]], mentors: List[List[int]]) -> int:
        m = len(students)
        n = len(students[0])
        memo = {}

        # Precompute scores between student i and mentor j
        scores = [[0] * m for _ in range(m)]
        for i in range(m):
            for j in range(m):
                scores[i][j] = sum(1 for k in range(n) if students[i][k] == mentors[j][k])

        def dp(student_idx, mentor_mask):
            if student_idx == m:
                return 0
            state = (student_idx, mentor_mask)
            if state in memo: return memo[state]

            max_score = 0
            for j in range(m):
                if not (mentor_mask & (1 << j)):
                    max_score = max(max_score, scores[student_idx][j] + dp(student_idx + 1, mentor_mask | (1 << j)))

            memo[state] = max_score
            return max_score

        return dp(0, 0)
```

### Complexity
- **Time:** $O(M^2 \cdot 2^M)$
- **Space:** $O(2^M)$

---

## 3. Find the Shortest Superstring

### Intuition
Given an array of strings `words`, return the smallest string that contains each string in `words` as a substring.
This is similar to TSP. We want to find an ordering of words with maximum overlap.
Precompute `overlap[i][j]`.
State: `dp[mask][last]` = max overlap using set `mask` ending with `words[last]`.

### Top-Down (Memoization)
```python
class Solution:
    def shortestSuperstring(self, words: List[str]) -> str:
        n = len(words)

        # Precompute overlaps
        overlaps = [[0] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if i == j: continue
                mn = min(len(words[i]), len(words[j]))
                for k in range(mn, 0, -1):
                    if words[i].endswith(words[j][:k]):
                        overlaps[i][j] = k
                        break

        memo = {}
        def dp(mask, last):
            if mask == (1 << n) - 1:
                return 0
            if (mask, last) in memo: return memo[(mask, last)]

            res = 0
            for curr in range(n):
                if not (mask & (1 << curr)):
                    # Overlap we gain by appending curr after last
                    # Note: Actually standard formulation is minimizing added length
                    # Or maximizing total overlap. Let's Maximize overlap.
                    val = overlaps[last][curr] + dp(mask | (1 << curr), curr)
                    res = max(res, val)

            memo[(mask, last)] = res
            return res

        # Reconstruct path (omitted for brevity, follows standard TSP reconstruction)
        # This problem is complex to code fully in short blocks, but core logic matches SOS/Bitmask.
        pass
```
*Note: While often classified under general Bitmask DP, SOS is specifically for subset sums. The first problem is the canonical SOS definition.*

### Alternative SOS Problem: OR and ADD
Given array A, compute `B[i] = Sum(A[j])` where `(i | j) == i` (i.e. j is subset of i).
This IS the definition of SOS DP as implemented in Problem 1.
The "Maximum Compatibility" is a matching problem on small constraints, fitting the "Advanced Bitmask" theme.

### Complexity
- **Time:** $O(N^2 2^N)$
- **Space:** $O(N 2^N)$
