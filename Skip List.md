# Skip List

> [!note] Description
> A skip list is a probabilistic data structure that allows $O(\log n)$ search complexity as well as $O(\log n)$ insertion complexity within an ordered sequence of elements. Thus it can get the best features of a sorted array (for searching) and a linked list (for insertion).

## Operations

### 1. Insert element
- **Complexity**: $O(\log n)$ average.

### 2. Delete element
- **Complexity**: $O(\log n)$ average.

### 3. Update element
- **Complexity**: $O(\log n)$ average.

### 4. Search element
- **Complexity**: $O(\log n)$ average.

### 5. Sort the elements
Skip list maintains elements in sorted order.
- **Complexity**: $O(n)$ to traverse.

## Complexity Summary

| Operation | Time Complexity (Average) | Time Complexity (Worst) |
| :--- | :--- | :--- |
| Access | $O(\log n)$ | $O(n)$ |
| Search | $O(\log n)$ | $O(n)$ |
| Insertion | $O(\log n)$ | $O(n)$ |
| Deletion | $O(\log n)$ | $O(n)$ |

## Implementation Example (Python)

```python
# Simplified concept
# In real implementation, need multiple levels with probabilistic promotion
class SkipNode:
    def __init__(self, val, level):
        self.val = val
        self.forward = [None] * (level + 1)
```
