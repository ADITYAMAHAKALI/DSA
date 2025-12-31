# Quick Sort

> [!note] Description
> QuickSort is a [[Divide and Conquer]] algorithm. It picks an element as a pivot and partitions the given array around the picked pivot.

## Intuition
The core idea is to select a "pivot" element and partition the array such that all elements smaller than the pivot are on its left and all elements larger are on its right. Once the pivot is in its correct sorted position, we recursively apply the same logic to the left and right subarrays. This "divide and conquer" strategy rapidly sorts the array without needing auxiliary space for merging.

## Complexity
- **Time Complexity**: $O(n \log n)$ (Average), $O(n^2)$ (Worst).
- **Space Complexity**: $O(\log n)$ (stack space).

## Implementation (Python)

```python
def partition(arr, low, high):
    i = (low - 1)
    pivot = arr[high]

    for j in range(low, high):
        if arr[j] <= pivot:
            i = i + 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return (i + 1)

def quick_sort(arr, low, high):
    if len(arr) == 1:
        return arr
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)
```

## Related Algorithms
- **Comparison**: [[Merge Sort]] (Stable, guaranteed $O(n \log n)$), [[Heap Sort]] (Guaranteed $O(n \log n)$, $O(1)$ space).
- **Paradigm**: [[Divide and Conquer]].
