# Combinatorics and Probability

Counting arrangements, selections, and paths.

## Problems

### 1. Unique Paths
**Problem**: Paths from top-left to bottom-right in $m \times n$ grid moving only down/right.
**Intuition**: Total moves = $(m-1) + (n-1)$. We need to choose $(m-1)$ down moves (or right moves) out of total.
**Formula**: $C(m+n-2, m-1)$.

```python
import math

def unique_paths(m, n):
    return math.comb(m + n - 2, m - 1)
```

### 2. Pascal's Triangle
**Problem**: Generate first `numRows`.
**Intuition**: $val[i][j] = val[i-1][j-1] + val[i-1][j]$.

```python
def generate_pascals(num_rows):
    triangle = []
    for i in range(num_rows):
        row = [1] * (i + 1)
        for j in range(1, i):
            row[j] = triangle[i-1][j-1] + triangle[i-1][j]
        triangle.append(row)
    return triangle
```

### 3. Factorial Trailing Zeroes
**Problem**: Count trailing zeroes in $n!$.
**Intuition**: Count factors of 5 in $n!$. (Since $2 \times 5 = 10$, and 2s are abundant). Count multiples of 5, 25, 125...

```python
def trailing_zeroes(n):
    count = 0
    while n > 0:
        n //= 5
        count += n
    return count
```
