# Bit Manipulation

> [!note] Description
> Bit manipulation involves direct manipulation of bits in binary representation of data. It is crucial for low-level programming, optimization, and solving specific algorithmic problems efficiently (often in $O(1)$ space).

## Intuition
The core intuition is that computers operate in binary. Manipulating these bits directly using operators like AND, OR, XOR, NOT, and Shifts can be incredibly fast and can solve problems that would otherwise require more complex data structures.

For instance, checking if a number is even or odd is traditionally done with modulo (`n % 2`), but `n & 1` is faster and operates directly on the least significant bit. Similarly, multiplying by 2 is just a left shift.

## Core Concepts & Operators

### 1. AND (`&`)
- **Rule**: Returns 1 if both bits are 1.
- **Properties**:
  - $x \ \& \ 0 = 0$ (Masking/Clearing)
  - $x \ \& \ x = x$ (Idempotent)
  - $x \ \& \ \text{all\_ones} = x$

### 2. OR (`|`)
- **Rule**: Returns 1 if at least one bit is 1.
- **Properties**:
  - $x \ | \ 0 = x$
  - $x \ | \ x = x$
  - $x \ | \ \text{all\_ones} = \text{all\_ones}$

### 3. XOR (`^`)
- **Rule**: Returns 1 if bits are different. (Also known as addition without carry).
- **Properties**:
  - $x \oplus 0 = x$ (Identity)
  - $x \oplus x = 0$ (Self-inverse)
  - $x \oplus y = y \oplus x$ (Commutative)
  - $(x \oplus y) \oplus z = x \oplus (y \oplus z)$ (Associative)
- **Key Application**: In-place swapping, finding unique elements.

### 4. NOT (`~`)
- **Rule**: Inverts all bits.
- **Note**: In Python (and other languages using 2's complement), `~x` is equivalent to `-x - 1`.

### 5. Left Shift (`<<`)
- **Rule**: Shifts bits to the left, filling with 0s.
- **Math**: $x \ll k$ is equivalent to $x \times 2^k$.
- **Example**: `3 << 2` $\rightarrow$ `0011` becomes `1100` (12). $3 \times 2^2 = 12$.

### 6. Right Shift (`>>`)
- **Rule**: Shifts bits to the right.
- **Math**: $x \gg k$ is equivalent to $\lfloor x / 2^k \rfloor$.
- **Example**: `12 >> 2` $\rightarrow$ `1100` becomes `0011` (3). $12 / 2^2 = 3$.

## Common Tricks & Techniques

### Basic Checks
- **Check Odd**: `(n & 1) == 1`
- **Check Even**: `(n & 1) == 0`
- **Check if Power of 2**: `n > 0 and (n & (n - 1) == 0)` (Powers of 2 have exactly one bit set).

### Bit Manipulation Tricks
- **Set $k$-th bit**: `n | (1 << k)`
- **Unset $k$-th bit**: `n & ~(1 << k)`
- **Toggle $k$-th bit**: `n ^ (1 << k)`
- **Get lowest set bit**: `n & -n` (Extracts the rightmost 1).
- **Clear lowest set bit**: `n & (n - 1)` (Turns the rightmost 1 into 0).

### XOR Swapping
Swap `a` and `b` without a temporary variable:
```python
a = a ^ b
b = a ^ b  # b becomes original a
a = a ^ b  # a becomes original b
```

## Example Questions

### 1. Single Number
**Problem**: Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.
**Intuition**: Use XOR properties. Pairs cancel out ($a \oplus a = 0$).

```python
def single_number(nums):
    res = 0
    for n in nums:
        res ^= n
    return res
```

### 2. Number of 1 Bits (Hamming Weight)
**Problem**: Count the number of '1' bits in an integer.
**Intuition**: Instead of checking every bit, use `n & (n - 1)` to repeatedly clear the least significant bit. This runs in $O(k)$ where $k$ is the number of set bits.

```python
def hamming_weight(n):
    count = 0
    while n:
        n &= (n - 1)
        count += 1
    return count
```

### 3. Counting Bits
**Problem**: Given $n$, return an array where `ans[i]` is the number of 1s in the binary representation of $i$ for $0 \le i \le n$.
**Intuition**: Use Dynamic Programming. The number of bits in $i$ is related to $i \gg 1$.
- If $i$ is even: `bits(i) = bits(i >> 1)` (LSB is 0).
- If $i$ is odd: `bits(i) = bits(i >> 1) + 1` (LSB is 1).
Combined: `dp[i] = dp[i >> 1] + (i & 1)`.

```python
def count_bits(n):
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i >> 1] + (i & 1)
    return dp
```

### 4. Sum of Two Integers
**Problem**: Calculate the sum of two integers `a` and `b` without using `+` or `-`.
**Intuition**:
- **Sum** (without carry) is `a ^ b`.
- **Carry** is `(a & b) << 1`.
Repeat until carry is 0.

```python
def get_sum(a, b):
    # 32-bit mask to handle Python's infinite large integers
    mask = 0xFFFFFFFF

    while b != 0:
        # a ^ b is sum without carry
        # (a & b) << 1 is carry
        sum_without_carry = (a ^ b) & mask
        carry = ((a & b) << 1) & mask

        a = sum_without_carry
        b = carry

    # If a is negative in 32-bit sense, converting it to Python's negative
    if (a >> 31) & 1:
        return ~(a ^ mask)
    return a
```

### 5. Reverse Bits
**Problem**: Reverse bits of a 32-bit unsigned integer.
**Intuition**: Iterate 32 times. Extract the last bit of `n` (`n & 1`) and push it to the result (`res = res << 1 | bit`).

```python
def reverse_bits(n):
    res = 0
    for _ in range(32):
        res = (res << 1) + (n & 1)
        n >>= 1
    return res
```

### 6. Subsets (Power Set)
**Problem**: Generate all subsets of a set.
**Intuition**: Iterate from $0$ to $2^n - 1$. The bitmask of the counter tells which elements to include.

```python
def subsets(nums):
    n = len(nums)
    output = []

    for i in range(1 << n):
        subset = []
        for j in range(n):
            if (i >> j) & 1:
                subset.append(nums[j])
        output.append(subset)
    return output
```

## Related Algorithms
- **Dynamic Programming**: [[Bitmasking DP]]
- **Data Structures**: [[Bloom Filter]], [[Fenwick Tree (Binary Indexed Tree)]]
