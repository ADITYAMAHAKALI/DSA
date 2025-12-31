# Bubble Sort

> [!note] Description
> Bubble Sort is the simplest sorting algorithm that works by repeatedly swapping the adjacent elements if they are in wrong order.

## Complexity
- **Time Complexity**: $O(n^2)$ (Worst/Average), $O(n)$ (Best).
- **Space Complexity**: $O(1)$.

## Implementation (Python)

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:
            break
```
