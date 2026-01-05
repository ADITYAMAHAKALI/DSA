# Math Algorithms

> [!note] Description
> Math algorithms in DSA cover fundamental mathematical concepts required to solve computational problems efficiently. These include number theory, combinatorics, geometry, and numerical methods.

## Intuition
Many algorithmic problems are disguised math problems. Recognizing the underlying mathematical property (like prime factorization, modular arithmetic rules, or geometric properties) allows for solutions that are exponentially faster than brute force.

## Topics & Patterns

The following pages cover specific patterns and problem sets in depth:

- [[math/Primes and Divisors|Primes and Divisors]]
    - Sieve of Eratosthenes
    - Prime Factorization
    - Ugly Numbers, Perfect Squares.

- [[math/Number Theory Basics|Number Theory Basics]]
    - GCD, LCM, Euclidean Algorithm
    - Modular Arithmetic
    - Digit manipulation (Palindromes, Happy Numbers).

- [[math/Combinatorics and Probability|Combinatorics and Probability]]
    - Permutations and Combinations ($nCr$)
    - Pascal's Triangle
    - Factorial problems.

- [[math/Geometric Algorithms|Geometric Algorithms]]
    - Coordinate geometry basics.
    - Rectangle Overlap, Valid Square, Points on a Line.

## Core Concepts Summary

### GCD and LCM
- **GCD**: $GCD(a, b) = GCD(b, a \pmod b)$
- **LCM**: $LCM(a, b) = (a \cdot b) / GCD(a, b)$

### Modulo Arithmetic
- $(a + b) \pmod m = ((a \pmod m) + (b \pmod m)) \pmod m$
- $(a \cdot b) \pmod m = ((a \pmod m) \cdot (b \pmod m)) \pmod m$

## Related Algorithms
- **Bit Manipulation**: [[Bit Manipulation]]
- **Dynamic Programming**: [[Catalan Numbers]]
