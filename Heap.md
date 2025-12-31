# Heap

> [!note] Description
> A Heap is a special Tree-based data structure in which the tree is a complete binary tree. Generally, Heaps can be of two types:
> - **Max-Heap**: The key present at the root node must be greatest among the keys present at all of its children.
> - **Min-Heap**: The key present at the root node must be minimum among the keys present at all of its children.

## Operations

### 1. Insert element
- **Complexity**: $O(\log n)$.

### 2. Delete element (Extract Max/Min)
- **Complexity**: $O(\log n)$.

### 3. Update element (Decrease/Increase Key)
- **Complexity**: $O(\log n)$.

### 4. Search element
- **Complexity**: $O(n)$ (Heaps are not optimized for searching arbitrary elements).

### 5. Sort the elements
- **Complexity**: $O(n \log n)$ ([[Heap Sort]]).

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Access Max/Min | $O(1)$ |
| Search | $O(n)$ |
| Insertion | $O(\log n)$ |
| Deletion (Max/Min) | $O(\log n)$ |

## Implementation Example (Python)

```python
import heapq

# Python's heapq implements a Min-Heap
h = []

# 1. Insert
heapq.heappush(h, 10)
heapq.heappush(h, 1)
heapq.heappush(h, 5)

# 2. Delete (Pop Min)
print(heapq.heappop(h)) # 1

# Peek Min
print(h[0]) # 5

# 3. Update (Decrease Key - tricky with standard heapq, need index)
# Standard way: delete old, insert new OR re-heapify
# Simple approach:
h[0] = 2
heapq.heapify(h) # O(n)

# 4. Search
if 10 in h:
    print("Found")

# 5. Sort (Heap Sort)
sorted_arr = []
while h:
    sorted_arr.append(heapq.heappop(h))
```

## Related Concepts
- [[Priority Queue]]: Abstract data type often implemented using a Heap.
- [[Heap Sort]]: Sorting algorithm using Heap.
- [[Graph]]: Heaps are used in graph algorithms like [[Dijkstra's Algorithm]] and [[Prim's Algorithm]].
