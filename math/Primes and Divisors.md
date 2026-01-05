# Primes and Divisors

Fundamental algorithms dealing with prime numbers and divisors.

## Core Algorithms
- **Sieve of Eratosthenes**: Finds all primes up to `n` in $O(n \log \log n)$.
- **Prime Factorization**: Breaking a number into product of primes.

## Problems

### 1. Count Primes
**Problem**: Count primes less than `n`.
**Intuition**: Use Sieve of Eratosthenes.

```python
def count_primes(n):
    if n <= 2: return 0
    prime = [True] * n
    prime[0] = prime[1] = False

    for i in range(2, int(n**0.5) + 1):
        if prime[i]:
            for j in range(i*i, n, i):
                prime[j] = False
    return sum(prime)
```

### 2. Ugly Number
**Problem**: Check if a number's prime factors are limited to 2, 3, and 5.
**Intuition**: Repeatedly divide by 2, 3, 5. If result is 1, it's ugly.

```python
def is_ugly(n):
    if n <= 0: return False
    for p in [2, 3, 5]:
        while n % p == 0:
            n //= p
    return n == 1
```

### 3. Ugly Number II
**Problem**: Find the $n$-th ugly number.
**Intuition**: Merge three sorted lists (multiples of 2, 3, 5). Keep pointers for 2, 3, and 5.

```python
def nth_ugly_number(n):
    nums = [1]
    i2, i3, i5 = 0, 0, 0
    while len(nums) < n:
        next_val = min(nums[i2]*2, nums[i3]*3, nums[i5]*5)
        nums.append(next_val)
        if next_val == nums[i2]*2: i2 += 1
        if next_val == nums[i3]*3: i3 += 1
        if next_val == nums[i5]*5: i5 += 1
    return nums[-1]
```

### 4. Perfect Squares
**Problem**: Least number of perfect squares that sum to `n`.
**Intuition**: BFS or DP (Knapsack-like). Or math (Legendre's 3-square theorem).
**DP Approach**: $dp[i] = \min(dp[i - j*j]) + 1$.

```python
def num_squares(n):
    dp = [float('inf')] * (n + 1)
    dp[0] = 0

    for i in range(1, n + 1):
        j = 1
        while j*j <= i:
            dp[i] = min(dp[i], dp[i - j*j] + 1)
            j += 1
    return dp[n]
```
