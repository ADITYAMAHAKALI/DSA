# Bubble Sort

> [!note] Description
> Bubble Sort is the simplest sorting algorithm that works by repeatedly swapping the adjacent elements if they are in wrong order.

## Intuition
The intuition is to "bubble" the largest elements to the top (end) of the list. We iterate through the list, comparing adjacent pairs. If they are in the wrong order, we swap them. After one full pass, the largest element is guaranteed to be in its correct final position. We repeat this for the remaining unsorted portion. If we go through a pass with no swaps, the list is already sorted.

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
