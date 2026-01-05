# Bitwise Basics

This section covers the fundamental operators and common basic problems involving bit manipulation.

## Core Operators

| Operator | Symbol | Description | Example (a=5 `101`, b=3 `011`) |
| :--- | :--- | :--- | :--- |
| AND | `&` | 1 if both bits are 1 | `a & b` = 1 (`001`) |
| OR | `\|` | 1 if either bit is 1 | `a \| b` = 7 (`111`) |
| XOR | `^` | 1 if bits are different | `a ^ b` = 6 (`110`) |
| NOT | `~` | Inverts bits | `~a` = -6 (in 2's complement) |
| Left Shift | `<<` | Shift left by bits (multiply by $2^k$) | `a << 1` = 10 (`1010`) |
| Right Shift | `>>` | Shift right by bits (divide by $2^k$) | `a >> 1` = 2 (`0010`) |

## Basic Problems

### 1. Number of 1 Bits (Hamming Weight)
**Problem**: Write a function that takes an unsigned integer and returns the number of '1' bits it has.
**Intuition**: Use `n & (n - 1)` to repeatedly clear the lowest set bit. This is faster than checking every bit.

```python
def hamming_weight(n):
    count = 0
    while n:
        n &= (n - 1)
        count += 1
    return count
```

### 2. Counting Bits
**Problem**: Given $n$, return an array where `ans[i]` is the number of 1s in $i$ for $0 \le i \le n$.
**Intuition**: DP approach. $bits(i) = bits(i >> 1) + (i \& 1)$.

```python
def count_bits(n):
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i >> 1] + (i & 1)
    return dp
```

### 3. Reverse Bits
**Problem**: Reverse bits of a 32-bit unsigned integer.
**Intuition**: Iterate 32 times. Extract the last bit of `n` and push it to `res`.

```python
def reverse_bits(n):
    res = 0
    for _ in range(32):
        res = (res << 1) + (n & 1)
        n >>= 1
    return res
```

### 4. Power of Two
**Problem**: Check if an integer is a power of two.
**Intuition**: Powers of two have exactly one bit set in binary representation. So `n & (n - 1)` should be 0. (Also n > 0).

```python
def is_power_of_two(n):
    return n > 0 and (n & (n - 1) == 0)
```
