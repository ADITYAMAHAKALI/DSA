# Binary Search

> [!note] Description
> Binary Search is a searching algorithm used in a sorted array by repeatedly dividing the search interval in half. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to $O(\log n)$.

## Complexity
- **Time Complexity**: $O(\log n)$.
- **Space Complexity**: $O(1)$ (iterative) or $O(\log n)$ (recursive).

## Implementation (Python)

```python
def binary_search(arr, target):
    low = 0
    high = len(arr) - 1

    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return -1
```
