# Backtracking

> [!note] Description
> Backtracking is an algorithmic-technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, removing those solutions that fail to satisfy the constraints of the problem at any point of time (by time, here, is referred to the time elapsed till reaching any level of the search tree).

## Intuition
The intuition is "trial and error." Imagine you are in a maze. You choose a path and follow it. If you hit a dead end, you "backtrack" to the last choice point and try a different path. Backtracking systematically explores all possible configurations (the search space), but it optimizes by pruning paths that are guaranteed to fail (bounding), saving vast amounts of time compared to brute force.

## Example: N-Queens Problem
```python
def is_safe(board, row, col, N):
    # Check this row on left side
    for i in range(col):
        if board[row][i] == 1:
            return False
    # Check upper diagonal on left side
    for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    # Check lower diagonal on left side
    for i, j in zip(range(row, N, 1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    return True

def solve_nq_util(board, col, N):
    if col >= N:
        return True
    for i in range(N):
        if is_safe(board, i, col, N):
            board[i][col] = 1
            if solve_nq_util(board, col + 1, N):
                return True
            board[i][col] = 0 # Backtrack
    return False
```
