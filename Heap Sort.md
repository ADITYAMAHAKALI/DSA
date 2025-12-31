# Heap Sort

> [!note] Description
> Heap sort is a comparison-based sorting technique based on Binary Heap data structure. It is similar to selection sort where we first find the minimum element and place the minimum element at the beginning. We repeat the same process for the remaining elements.

## Intuition
Heap Sort treats the array as a binary heap (a tree structure where parent $\ge$ children). By first building a max-heap, we ensure the largest element is at the root. We swap this root with the last element of the unsorted portion, effectively placing the largest element in its correct sorted position at the end. We then reduce the heap size and "heapify" the root to maintain the max-heap property. Repeating this process moves the next largest element to the end, and so on, resulting in a sorted array.

## Complexity
- **Time Complexity**: $O(n \log n)$.
- **Space Complexity**: $O(1)$.

## Implementation (Python)

```python
def heapify(arr, n, i):
    largest = i
    l = 2 * i + 1
    r = 2 * i + 2

    if l < n and arr[i] < arr[l]:
        largest = l

    if r < n and arr[largest] < arr[r]:
        largest = r

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

def heap_sort(arr):
    n = len(arr)

    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)
```
