# Bit Manipulation

> [!note] Description
> Bit manipulation involves direct manipulation of bits in binary representation of data. It is crucial for low-level programming, optimization, and solving specific algorithmic problems efficiently (often in $O(1)$ space).

## Intuition
The core intuition is that computers operate in binary. Manipulating these bits directly using operators like AND, OR, XOR, NOT, and Shifts can be incredibly fast and can solve problems that would otherwise require more complex data structures. For example, using XOR to find a unique element is based on the property $x \oplus x = 0$.

## Core Concepts

### Bitwise Operators
- **AND (`&`)**: Returns 1 if both bits are 1.
- **OR (`|`)**: Returns 1 if at least one bit is 1.
- **XOR (`^`)**: Returns 1 if bits are different.
- **NOT (`~`)**: Inverts bits.
- **Left Shift (`<<`)**: Shifts bits to the left (multiplies by 2).
- **Right Shift (`>>`)**: Shifts bits to the right (divides by 2).

### Common Tricks
- **Check if $k$-th bit is set**: `(n >> k) & 1`
- **Set $k$-th bit**: `n | (1 << k)`
- **Unset $k$-th bit**: `n & ~(1 << k)`
- **Toggle $k$-th bit**: `n ^ (1 << k)`
- **Check if power of 2**: `n > 0 and (n & (n - 1) == 0)`
- **Clear lowest set bit**: `n & (n - 1)`
- **Get lowest set bit**: `n & -n`

## Example Questions

### 1. Single Number
**Problem**: Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.
**Intuition**: Use XOR. Since $a \oplus a = 0$ and $a \oplus 0 = a$, XORing all numbers together will cancel out the pairs, leaving the unique number.

```python
def single_number(nums):
    res = 0
    for n in nums:
        res ^= n
    return res
```

### 2. Number of 1 Bits (Hamming Weight)
**Problem**: Write a function that takes an unsigned integer and returns the number of '1' bits it has.
**Intuition**: Repeatedly clear the lowest set bit using `n & (n - 1)` until `n` becomes 0. Count the operations.

```python
def hamming_weight(n):
    count = 0
    while n:
        n &= (n - 1)
        count += 1
    return count
```

### 3. Subsets (Power Set)
**Problem**: Given an integer array `nums` of unique elements, return all possible subsets.
**Intuition**: A set of size $n$ has $2^n$ subsets. Each subset corresponds to a binary representation of length $n$, where the $i$-th bit determines if the $i$-th element is included.

```python
def subsets(nums):
    n = len(nums)
    output = []

    for i in range(1 << n):
        # generate subset for pattern i
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
- **Math**: [[Math Algorithms]] (Binary representation relates to base-2 math)
