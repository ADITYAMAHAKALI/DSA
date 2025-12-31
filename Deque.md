# Deque

> [!note] Description
> Deque or Double Ended Queue is a generalized version of Queue data structure that allows insert and delete at both ends.

## Operations

### 1. Insert element (Front/Rear)
- **Complexity**: $O(1)$.

### 2. Delete element (Front/Rear)
- **Complexity**: $O(1)$.

### 3. Update element
- **Complexity**: $O(n)$ or $O(1)$ if index based (implementation dependent).

### 4. Search element
- **Complexity**: $O(n)$.

### 5. Sort the elements
- **Complexity**: $O(n \log n)$.

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Insertion (Front/Rear) | $O(1)$ |
| Deletion (Front/Rear) | $O(1)$ |
| Access | $O(n)$ (Linked List) / $O(1)$ (Array) |

## Implementation Example (Python)

```python
from collections import deque

d = deque()

# 1. Insert
d.append(1)        # Rear
d.appendleft(2)    # Front

# 2. Delete
d.pop()            # Rear
d.popleft()        # Front
```
