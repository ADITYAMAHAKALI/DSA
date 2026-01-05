# Math Algorithms

> [!note] Description
> Math algorithms in DSA cover fundamental mathematical concepts required to solve computational problems efficiently. These include number theory, combinatorics, geometry, and numerical methods.

## Intuition
Many algorithmic problems are disguised math problems. Recognizing the underlying mathematical property (like prime factorization, modular arithmetic rules, or geometric properties) allows for solutions that are exponentially faster than brute force. For instance, finding divisors up to $\sqrt{n}$ instead of $n$ reduces complexity from $O(n)$ to $O(\sqrt{n})$.

## Core Concepts

### 1. GCD and LCM
- **GCD (Greatest Common Divisor)**: The largest positive integer that divides each of the integers.
    - Algorithm: Euclidean Algorithm ($GCD(a, b) = GCD(b, a \pmod b)$).
- **LCM (Least Common Multiple)**: The smallest positive integer that is divisible by both integers.
    - Formula: $LCM(a, b) = \frac{|a \cdot b|}{GCD(a, b)}$.

### 2. Prime Numbers & Factorization
- **Primality Test**: Check if a number is prime. Iterate $2$ to $\sqrt{n}$.
- **Sieve of Eratosthenes**: Efficiently finds all primes up to a limit $n$. Time complexity: $O(n \log \log n)$.
- **Prime Factorization**: Every integer $>1$ is either prime or a product of primes.
    - Fundamental Theorem of Arithmetic.
    - Algorithm: Divide by 2 until odd, then divide by odd numbers $i$ starting from 3 up to $\sqrt{n}$.

### 3. Modular Arithmetic
Essential for problems involving large numbers or cyclic patterns.
- **Properties**:
    - $(a + b) \pmod m = ((a \pmod m) + (b \pmod m)) \pmod m$
    - $(a \cdot b) \pmod m = ((a \pmod m) \cdot (b \pmod m)) \pmod m$
    - **Modular Inverse**: Used for division modulo $m$. Requires Extended Euclidean Algorithm.
    - **Modular Exponentiation**: Computing $a^b \pmod m$ efficiently in $O(\log b)$.

### 4. Combinatorics
- **Permutations ($nPr$)**: Arrangements of items where order matters. $P(n, k) = \frac{n!}{(n-k)!}$.
- **Combinations ($nCr$)**: Selections of items where order doesn't matter. $C(n, k) = \frac{n!}{k!(n-k)!}$.
- **Pascal's Triangle**: Used to calculate binomial coefficients $C(n, k)$ using DP.

## Common Interview Problems

### 1. Excel Sheet Column Title
**Problem**: Convert a column number (1 -> "A", 28 -> "AB") to its title.
**Intuition**: This is effectively a base-26 conversion problem, but 1-indexed (no 0).
**Key**: Perform `(n - 1) % 26` to map 1..26 to 0..25 ('A'..'Z').

### 2. Factorial Trailing Zeroes
**Problem**: Count trailing zeroes in $n!$.
**Intuition**: A zero is formed by $2 \times 5$. There are always more factors of 2 than 5. Thus, we just count the number of factors of 5 in $n!$.
**Technique**: Count multiples of 5, 25, 125... in $n$.

### 3. Happy Number
**Problem**: Determine if a number is "happy" (sum of squares of digits repeats until it hits 1 or loops).
**Intuition**: This is a cycle detection problem.
**Technique**: Use a HashSet to detect loops, or Floyd's Cycle Finding (Fast & Slow pointers).

### 4. Sqrt(x)
**Problem**: Compute integer square root of $x$.
**Intuition**: The square root is monotonic.
**Technique**: Use Binary Search on the range $[0, x]$.

### 5. Palindrome Number
**Problem**: Check if an integer is a palindrome without converting to string.
**Intuition**: Reconstruct the reversed number mathematically using modulo and division.
**Check**: `x == reversed_x` (handle negative cases).

## Code Examples

### 1. Greatest Common Divisor (GCD)
```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a
```

### 2. Count Primes (Sieve of Eratosthenes)
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

### 3. Power(x, n) (Binary Exponentiation)
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

### 4. Prime Factorization
```python
def get_prime_factors(n):
    factors = []
    # Handle 2 separately
    while n % 2 == 0:
        factors.append(2)
        n //= 2

    # Check odd numbers
    for i in range(3, int(n**0.5) + 1, 2):
        while n % i == 0:
            factors.append(i)
            n //= i

    if n > 2:
        factors.append(n)
    return factors
```

### 5. Combinations (nCr)
```python
def nCr(n, r):
    if r < 0 or r > n:
        return 0
    if r == 0 or r == n:
        return 1
    if r > n // 2:
        r = n - r

    res = 1
    for i in range(r):
        res = res * (n - i) // (i + 1)
    return res
```

## Related Algorithms
- **Bit Manipulation**: [[Bit Manipulation]] (Binary Exponentiation uses bits).
- **Dynamic Programming**: [[Catalan Numbers]] (Combinatorics), [[Dynamic Programming]] (Pascal's Triangle).
- **Graph Algorithms**: Cycle detection (Happy Number related).
