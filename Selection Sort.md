# Selection Sort

> [!note] Description
> The selection sort algorithm sorts an array by repeatedly finding the minimum element (considering ascending order) from unsorted part and putting it at the beginning.

## Intuition
The intuition is to build the sorted list one element at a time by finding the "next smallest" element. We divide the list into a sorted part (left) and an unsorted part (right). In each iteration, we scan the entire unsorted part to find the absolute minimum value and swap it with the first element of the unsorted part. This expands the sorted part by one.

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
