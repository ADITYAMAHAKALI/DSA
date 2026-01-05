# Math Algorithms

> [!note] Description
> Math algorithms in DSA cover fundamental mathematical concepts required to solve computational problems efficiently. These include number theory, combinatorics, geometry, and numerical methods.

## Intuition
Many algorithmic problems are disguised math problems. Recognizing the underlying mathematical property (like prime factorization, modular arithmetic rules, or geometric properties) allows for solutions that are exponentially faster than brute force. For instance, finding divisors up to $\sqrt{n}$ instead of $n$ reduces complexity from $O(n)$ to $O(\sqrt{n})$.

## Core Concepts

### GCD and LCM
- **GCD (Greatest Common Divisor)**: The largest positive integer that divides each of the integers. Computed efficiently using the Euclidean algorithm.
- **LCM (Least Common Multiple)**: The smallest positive integer that is divisible by both integers. $LCM(a, b) = \frac{|a \cdot b|}{GCD(a, b)}$.

### Prime Numbers
- **Primality Test**: Check if a number is prime. Basic check is $O(\sqrt{n})$.
- **Sieve of Eratosthenes**: Efficiently finds all primes up to a limit $n$. Time complexity: $O(n \log \log n)$.

### Modular Arithmetic
- Properties:
    - $(a + b) \pmod m = ((a \pmod m) + (b \pmod m)) \pmod m$
    - $(a \cdot b) \pmod m = ((a \pmod m) \cdot (b \pmod m)) \pmod m$
    - Modular Exponentiation: Computing $a^b \pmod m$ efficiently in $O(\log b)$.

## Example Questions

### 1. Greatest Common Divisor (GCD)
**Problem**: Find the GCD of two numbers.
**Intuition**: Euclidean algorithm: $GCD(a, b) = GCD(b, a \pmod b)$ until $b = 0$.

```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a
```

### 2. Count Primes (Sieve of Eratosthenes)
**Problem**: Count the number of prime numbers less than a non-negative number $n$.
**Intuition**: Mark multiples of each prime starting from 2.

```python
def count_primes(n):
    if n <= 2:
        return 0

    is_prime = [True] * n
    is_prime[0] = is_prime[1] = False

    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n, i):
                is_prime[j] = False

    return sum(is_prime)
```

### 3. Power(x, n)
**Problem**: Implement `pow(x, n)`, which calculates $x$ raised to the power $n$ ($x^n$).
**Intuition**: Use Binary Exponentiation (Divide and Conquer). If $n$ is even, $x^n = (x^{n/2})^2$. If $n$ is odd, $x^n = x \cdot x^{n-1}$.

```python
def my_pow(x, n):
    if n < 0:
        x = 1 / x
        n = -n

    res = 1
    while n:
        if n % 2:
            res *= x
        x *= x
        n //= 2
    return res
```

## Related Algorithms
- **Cryptography**: heavily relies on number theory (RSA encryption uses large primes).
- **Bit Manipulation**: [[Bit Manipulation]] (Binary Exponentiation uses bits).
- **Dynamic Programming**: [[Catalan Numbers]] (Combinatorics).
- **Geometric Algorithms**: Convex Hull, Line Intersection.
