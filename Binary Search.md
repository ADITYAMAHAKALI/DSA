# Binary Search

> [!note] Description
> Binary Search is a searching algorithm used in a sorted [[Array]] by repeatedly dividing the search interval in half. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to $O(\log n)$.

## Intuition
The intuition behind Binary Search is similar to looking for a word in a dictionary. You open the book to the middle. If the word you're looking for comes after the word at the middle, you discard the first half of the book and look in the second half. You repeat this process, constantly halving the search space, until you find the word. This makes it exponentially faster than checking every single element.

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

## Related Data Structures
- [[Array]]: The data structure required for Binary Search (must be sorted).
- [[Binary Search Tree]]: A tree structure that uses a similar principle for searching (binary splitting).
- [[Skip List]]: A probabilistic data structure that allows $O(\log n)$ search, similar to binary search on a linked list.
