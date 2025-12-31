# Selection Sort

> [!note] Description
> The selection sort algorithm sorts an array by repeatedly finding the minimum element (considering ascending order) from unsorted part and putting it at the beginning.

## Complexity
- **Time Complexity**: $O(n^2)$.
- **Space Complexity**: $O(1)$.

## Implementation (Python)

```python
def selection_sort(arr):
    for i in range(len(arr)):
        min_idx = i
        for j in range(i + 1, len(arr)):
            if arr[min_idx] > arr[j]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
```
