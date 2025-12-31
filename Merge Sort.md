# Merge Sort

> [!note] Description
> Merge Sort is a Divide and Conquer algorithm. It divides the input array into two halves, calls itself for the two halves, and then merges the two sorted halves.

## Intuition
The intuition is that it is easier to sort two smaller arrays than one large array. Also, merging two already sorted arrays into a new sorted array is very efficient (linear time). By recursively splitting the array until we have subarrays of size 1 (which are inherently sorted) and then merging them back together in sorted order, we can sort the entire array efficiently.

## Complexity
- **Time Complexity**: $O(n \log n)$ (Worst, Average, Best).
- **Space Complexity**: $O(n)$ (for auxiliary array).

## Implementation (Python)

```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        L = arr[:mid]
        R = arr[mid:]

        merge_sort(L)
        merge_sort(R)

        i = j = k = 0

        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1

        while i < len(L):
            arr[k] = L[i]
            i += 1
            k += 1

        while j < len(R):
            arr[k] = R[j]
            j += 1
            k += 1
```
