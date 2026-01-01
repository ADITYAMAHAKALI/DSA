> [!note] Description
> Advanced Digit DP involves harder constraints, often combining Digit DP with state machines or other properties.

## 1. Non-negative Integers without Consecutive Ones

### Intuition
Given `n`, count non-negative integers $\le n$ without consecutive ones.
This is Digit DP. State `(index, tight, last_digit_was_one)`.
Alternatively, this is related to Fibonacci numbers if we just count valid strings of length $L$.

### Top-Down (Memoization)
```python
class Solution:
    def findIntegers(self, n: int) -> int:
        s = bin(n)[2:]
        memo = {}

        def dp(i, tight, prev_one):
            if i == len(s):
                return 1
            if (i, tight, prev_one) in memo:
                return memo[(i, tight, prev_one)]

            limit = int(s[i]) if tight else 1
            res = 0

            for digit in range(limit + 1):
                if prev_one and digit == 1:
                    continue

                new_tight = tight and (digit == limit)
                res += dp(i + 1, new_tight, digit == 1)

            memo[(i, tight, prev_one)] = res
            return res

        return dp(0, True, False)
```

### Complexity
- **Time:** $O(\log N)$
- **Space:** $O(\log N)$

---

## 2. Numbers With Repeated Digits

### Intuition
Given `n`, return number of positive integers $\le n$ that have at least one repeated digit.
Count Total ($N$) - Count Unique ($<= N$).
We solved "Count Unique" in Core Digit DP.

### Implementation
```python
class Solution:
    def numDupDigitsAtMostN(self, n: int) -> int:
        # Total numbers is n (1 to n)
        # We need to find Count(Unique <= n)

        s = str(n)
        k = len(s)
        memo = {}

        def dp(i, tight, mask, is_lead_zero):
            if i == k:
                return 1
            state = (i, tight, mask, is_lead_zero)
            if state in memo: return memo[state]

            limit = int(s[i]) if tight else 9
            res = 0

            for d in range(limit + 1):
                if is_lead_zero and d == 0:
                     # Still leading zero, mask is empty
                     res += dp(i + 1, tight and (d == limit), 0, True)
                else:
                    if mask & (1 << d): continue # Digit repeated
                    res += dp(i + 1, tight and (d == limit), mask | (1 << d), False)

            memo[state] = res
            return res

        # Result includes 0 (empty mask case effectively), but we want positive integers.
        # The DP as written counts valid sequences.
        # Actually, let's just use the result:
        unique_count = dp(0, True, 0, True)

        # unique_count will include the case of "all zeros" if we are not careful?
        # With is_lead_zero logic, 000... is counted as a valid number (value 0).
        # We want positive integers. So subtract 1 for the number 0.
        return n - (unique_count - 1)
```

### Complexity
- **Time:** $O(\log N \cdot 2^{10})$
- **Space:** $O(\log N \cdot 2^{10})$

---

## 3. Rotated Digits

### Intuition
A number is valid if after rotating each digit 180 degrees, it becomes a different number. Each digit must be valid.
Valid rotations: 0->0, 1->1, 8->8, 2->5, 5->2, 6->9, 9->6.
Invalid: 3, 4, 7.
A number is valid if:
1. It contains only {0, 1, 8, 2, 5, 6, 9}.
2. It contains at least one of {2, 5, 6, 9} (so it changes).
State: `(index, tight, changed)`.

### Top-Down (Memoization)
```python
class Solution:
    def rotatedDigits(self, n: int) -> int:
        s = str(n)
        memo = {}

        def dp(i, tight, changed):
            if i == len(s):
                return 1 if changed else 0
            state = (i, tight, changed)
            if state in memo: return memo[state]

            limit = int(s[i]) if tight else 9
            res = 0

            for d in range(limit + 1):
                if d in {3, 4, 7}:
                    continue

                new_changed = changed or (d in {2, 5, 6, 9})
                new_tight = tight and (d == limit)
                res += dp(i + 1, new_tight, new_changed)

            memo[state] = res
            return res

        return dp(0, True, False)
```

### Complexity
- **Time:** $O(\log N)$
- **Space:** $O(\log N)$
