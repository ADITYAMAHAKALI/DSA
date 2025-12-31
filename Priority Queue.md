# Priority Queue

> [!note] Description
> A Priority Queue is an abstract data type similar to a regular queue or stack data structure in which each element additionally has a "priority" associated with it. In a priority queue, an element with high priority is served before an element with low priority.

## Operations

### 1. Insert element
- **Complexity**: $O(\log n)$.

### 2. Delete element (Extract Max/Min)
- **Complexity**: $O(\log n)$.

### 3. Update element (Change Priority)
- **Complexity**: $O(\log n)$.

### 4. Search element
- **Complexity**: $O(n)$.

### 5. Sort the elements
- **Complexity**: $O(n \log n)$.

## Complexity Summary (Binary Heap)

| Operation | Time Complexity |
| :--- | :--- |
| Insertion | $O(\log n)$ |
| Deletion (Max/Min) | $O(\log n)$ |
| Peek | $O(1)$ |

## Implementation Example (Python)

```python
import heapq

# Python's heapq is a priority queue (min-heap)
pq = []

# 1. Insert
heapq.heappush(pq, (2, 'code'))
heapq.heappush(pq, (1, 'eat'))
heapq.heappush(pq, (3, 'sleep'))

# 2. Delete
while pq:
    item = heapq.heappop(pq)
    print(item)
# Output: (1, 'eat'), (2, 'code'), (3, 'sleep')
```
