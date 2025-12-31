# Insertion Sort

> [!note] Description
> Insertion sort is a simple sorting algorithm that works similar to the way you sort playing cards in your hands. The array is virtually split into a sorted and an unsorted part. Values from the unsorted part are picked and placed at the correct position in the sorted part.

## Complexity
- **Time Complexity**: $O(n^2)$ (Worst/Average), $O(n)$ (Best).
- **Space Complexity**: $O(1)$.

## Implementation (Python)

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```
