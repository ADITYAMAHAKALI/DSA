# Bitwise Arithmetic

This section covers problems where you need to perform arithmetic operations using bitwise operators, simulating hardware logic.

## Problems

### 1. Sum of Two Integers
**Problem**: Calculate `a + b` without `+` or `-`.
**Intuition**:
- `a ^ b`: Sum without carry.
- `(a & b) << 1`: Carry.
- Repeat until carry is 0. Note: Python requires manual 32-bit masking.

```python
def get_sum(a, b):
    mask = 0xFFFFFFFF
    while b != 0:
        sum_no_carry = (a ^ b) & mask
        carry = ((a & b) << 1) & mask
        a, b = sum_no_carry, carry

    # Handle negative numbers for Python
    if (a >> 31) & 1:
        return ~(a ^ mask)
    return a
```

### 2. Divide Two Integers
**Problem**: Divide two integers without `*`, `/`, `%`.
**Intuition**: Use subtraction based on powers of 2 (Shift). Like long division in binary.
Subtract `divisor * 2^k` from `dividend` where `k` is as large as possible.

```python
def divide(dividend, divisor):
    if dividend == -2**31 and divisor == -1:
        return 2**31 - 1

    a, b = abs(dividend), abs(divisor)
    res = 0

    # Iterate from 31 down to 0
    for x in range(31, -1, -1):
        if (a >> x) >= b:
            res += (1 << x)
            a -= (b << x)

    return res if (dividend > 0) == (divisor > 0) else -res
```

### 3. Maximum Product of Word Lengths
**Problem**: Given a string array `words`, find max `len(words[i]) * len(words[j])` where the two words share no common letters.
**Intuition**: Use a bitmask to represent the set of characters in each word (26 bits). Check intersection with `mask[i] & mask[j] == 0`.

```python
def max_product(words):
    n = len(words)
    masks = [0] * n
    lens = [0] * n

    for i, word in enumerate(words):
        lens[i] = len(word)
        for ch in word:
            masks[i] |= 1 << (ord(ch) - ord('a'))

    max_val = 0
    for i in range(n):
        for j in range(i + 1, n):
            if not (masks[i] & masks[j]):
                max_val = max(max_val, lens[i] * lens[j])

    return max_val
```
