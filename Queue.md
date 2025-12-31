# Queue

> [!note] Description
> A Queue is a linear structure which follows a particular order in which the operations are performed. The order is First In First Out (FIFO).

## Operations

### 1. Insert element (Enqueue)
Adds an item to the queue.
- **Complexity**: $O(1)$.

### 2. Delete element (Dequeue)
Removes an item from the queue. The items are popped in the same order in which they are pushed.
- **Complexity**: $O(1)$.

### 3. Update element
Generally not supported directly.

### 4. Search element
- **Complexity**: $O(n)$.

### 5. Sort the elements
- **Complexity**: $O(n \log n)$ (extracting to array or using priority queue).

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Access | $O(n)$ |
| Search | $O(n)$ |
| Insertion (Enqueue) | $O(1)$ |
| Deletion (Dequeue) | $O(1)$ |

## Implementation Example (Python)

```python
from collections import deque

queue = deque()

# 1. Insert (Enqueue)
queue.append('a')
queue.append('b')

# 2. Delete (Dequeue)
print(queue.popleft()) # 'a'

# 3. Update (Front element)
if queue:
    # Not directly supported in deque to update arbitrary index efficiently,
    # but we can update by accessing index
    queue[0] = 'z'

# 4. Search
if 'b' in queue:
    print("Found")

# 5. Sort
# deque does not have a sort method, convert to list
sorted_queue = sorted(list(queue))
```
