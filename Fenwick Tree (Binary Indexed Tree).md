# Fenwick Tree (Binary Indexed Tree)

> [!note] Description
> A Fenwick tree or binary indexed tree is a data structure that can efficiently update elements and calculate prefix sums in a table of numbers.

## Operations

### 1. Insert element (Build)
- **Complexity**: $O(n \log n)$ or $O(n)$.

### 2. Delete element
Not typical. Update inverse.

### 3. Update element
- **Complexity**: $O(\log n)$.

### 4. Search element (Prefix Sum)
- **Complexity**: $O(\log n)$.

### 5. Sort the elements
Not applicable.

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Update | $O(\log n)$ |
| Prefix Sum | $O(\log n)$ |
| Space | $O(n)$ |

## Implementation Example (Python)

```python
class FenwickTree:
    # 1. Insert (Init/Build)
    def __init__(self, size):
        self.tree = [0] * (size + 1)

    # 3. Update (Add delta to element at i)
    def update(self, i, delta):
        while i < len(self.tree):
            self.tree[i] += delta
            i += i & (-i)

    # 4. Search (Prefix Sum Query)
    def query(self, i):
        s = 0
        while i > 0:
            s += self.tree[i]
            i -= i & (-i)
        return s

    # 2. Delete (Simulate by adding negative value)
    def delete(self, i, val):
        self.update(i, -val)

    # 5. Sort (Not applicable for BIT itself, maintains partial sums)
```
