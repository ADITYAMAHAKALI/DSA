# Array

> [!note] Description
> An array is a collection of items, stored at contiguous memory locations. The idea is to store multiple items of the same type together.

## Operations

### 1. Insert element
Insertion in an array can be at the beginning, end, or at a given index.
- **At End**: $O(1)$ (amortized if dynamic array).
- **At Index/Beginning**: $O(n)$ because elements need to be shifted.

### 2. Delete element
Deletion involves removing an element and shifting subsequent elements to fill the gap.
- **At End**: $O(1)$.
- **At Index/Beginning**: $O(n)$.

### 3. Update element
Updating an element at a specific index is direct access.
- **Complexity**: $O(1)$.

### 4. Search element
- **By Index**: $O(1)$.
- **By Value (Linear Search)**: $O(n)$.
- **By Value (Binary Search - if sorted)**: $O(\log n)$.

### 5. Sort the elements
Sorting an array puts elements in a specific order (e.g., ascending).
- **Algorithms**: Quick Sort, Merge Sort, etc.
- **Complexity**: Typically $O(n \log n)$.

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Access | $O(1)$ |
| Search | $O(n)$ |
| Insertion | $O(n)$ |
| Deletion | $O(n)$ |

## Implementation Example (Python)

```python
# Python lists are dynamic arrays
arr = [1, 2, 3, 4, 5]

# 1. Insert
arr.append(6)       # End: [1, 2, 3, 4, 5, 6]
arr.insert(0, 0)    # Beginning: [0, 1, 2, 3, 4, 5, 6]

# 2. Delete
arr.pop()           # Remove last
arr.remove(3)       # Remove element '3'

# 3. Update
arr[0] = 10         # Update index 0

# 4. Search
index = arr.index(4) # Find index of value 4
val = arr[2]         # Access index 2

# 5. Sort
arr.sort()
```
