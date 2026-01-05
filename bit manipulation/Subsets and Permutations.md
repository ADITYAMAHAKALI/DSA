# Subsets and Permutations

Generating subsets and permutations using bitwise operations.

## Problems

### 1. Subsets (Power Set)
**Problem**: Generate all subsets.
**Intuition**: Iterate $0$ to $2^n - 1$. If $j$-th bit of $i$ is set, include $nums[j]$.

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

### 2. Gray Code
**Problem**: Sequence of $2^n$ integers where consecutive values differ by exactly one bit.
**Intuition**: $G(i) = i \oplus (i >> 1)$.

```python
def gray_code(n):
    res = []
    for i in range(1 << n):
        res.append(i ^ (i >> 1))
    return res
```
