# Bit Manipulation

> [!note] Description
> Bit manipulation involves direct manipulation of bits in binary representation of data. It is crucial for low-level programming, optimization, and solving specific algorithmic problems efficiently (often in $O(1)$ space).

## Intuition
The core intuition is that computers operate in binary. Manipulating these bits directly using operators like AND, OR, XOR, NOT, and Shifts can be incredibly fast and can solve problems that would otherwise require more complex data structures.

## Topics & Patterns

The following pages cover specific patterns and problem sets in depth:

- [[bit manipulation/Bitwise Basics|Bitwise Basics]]
    - Core operators (`&`, `|`, `~`, `<<`, `>>`)
    - Basic flags and checks (Odd/Even, Power of Two)
    - Essential problems like Counting Bits and Reverse Bits.

- [[bit manipulation/XOR Manipulation|XOR Manipulation]]
    - The "Magic" of XOR (Self-inverse, Commutative)
    - Solving "Single Number" and "Missing Number" variations.

- [[bit manipulation/Bitwise Arithmetic|Bitwise Arithmetic]]
    - Performing arithmetic operations (Sum, Divide) without standard operators.
    - Advanced masking and manipulation.

## Core Concepts Summary

### Bitwise Operators
- **AND (`&`)**: Both bits must be 1.
- **OR (`|`)**: At least one bit is 1.
- **XOR (`^`)**: Bits must be different.
- **NOT (`~`)**: Inverts bits.
- **Shifts (`<<`, `>>`)**: Multiply or Divide by powers of 2.

### Common Tricks
- **Check Odd**: `(n & 1) == 1`
- **Set $k$-th bit**: `n | (1 << k)`
- **Unset $k$-th bit**: `n & ~(1 << k)`
- **Toggle $k$-th bit**: `n ^ (1 << k)`
- **Lowest Set Bit**: `n & -n`
- **Clear Lowest Set Bit**: `n & (n - 1)`

## Related Algorithms
- **Dynamic Programming**: [[Bitmasking DP]]
- **Data Structures**: [[Bloom Filter]], [[Fenwick Tree (Binary Indexed Tree)]]
