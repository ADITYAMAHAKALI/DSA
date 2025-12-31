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

## Complexity Summary ([[Binary Heap]])

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

# 2. Delete (Pop Min)
# Note: This also effectively Sorts if done repeatedly
while pq:
    item = heapq.heappop(pq)
    print(item)
# Output: (1, 'eat'), (2, 'code'), (3, 'sleep')

# Re-populate for other operations
pq = [(2, 'code'), (1, 'eat'), (3, 'sleep')]
heapq.heapify(pq)

# 3. Update (Change Priority)
# Standard heapq doesn't support easy update.
# Workaround: Mark old as removed (lazy deletion) or remove and re-insert (O(n))
# Here is remove and re-insert approach:
pq.remove((2, 'code')) # O(n)
heapq.heappush(pq, (0, 'code')) # New priority 0

# 4. Search
if (1, 'eat') in pq:
    print("Found")

# 5. Sort
# Heap Sort logic:
sorted_list = []
temp_pq = pq[:] # Copy
while temp_pq:
    sorted_list.append(heapq.heappop(temp_pq))
```

## Related Data Structures
- [[Heap]]: The most common implementation of a Priority Queue.
- [[Queue]]: A similar structure but FIFO.
- [[Stack]]: A similar structure but LIFO.
- [[Graph]]: Used in algorithms like [[Dijkstra's Algorithm]], [[Prim's Algorithm]], [[A* Algorithm]].
