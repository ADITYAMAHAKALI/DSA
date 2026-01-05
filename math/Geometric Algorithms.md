# Geometric Algorithms

Problems involving coordinates, lines, and shapes.

## Problems

### 1. Valid Square
**Problem**: Given 4 points, check if they form a square.
**Intuition**: Calculate all 6 distances between points. For a square, there should be only 2 unique non-zero distances: side length and diagonal length.
Diagonal > Side.

```python
def valid_square(p1, p2, p3, p4):
    def dist(a, b):
        return (a[0]-b[0])**2 + (a[1]-b[1])**2

    points = [p1, p2, p3, p4]
    dists = []
    for i in range(4):
        for j in range(i + 1, 4):
            dists.append(dist(points[i], points[j]))

    dists.sort()
    # 4 equal sides, 2 equal diagonals
    return 0 < dists[0] == dists[1] == dists[2] == dists[3] and dists[4] == dists[5]
```

### 2. Rectangle Overlap
**Problem**: Check if two rectangles overlap. Rec1: $[x1, y1, x2, y2]$, Rec2: $[x3, y3, x4, y4]$.
**Intuition**: Check if they *don't* overlap (one is left, right, top, or bottom of other) and negate.

```python
def is_rectangle_overlap(rec1, rec2):
    # Check if rec1 is left, right, below, or above rec2
    if (rec1[2] <= rec2[0] or  # left
        rec1[0] >= rec2[2] or  # right
        rec1[3] <= rec2[1] or  # bottom
        rec1[1] >= rec2[3]):   # top
        return False
    return True
```

### 3. Max Points on a Line
**Problem**: Given $n$ points, find max points on the same straight line.
**Intuition**: For each point `i`, calculate slopes to all other points `j`. Use a map to count same slopes. Handle vertical lines and duplicate points.

```python
from collections import defaultdict
import math

def max_points(points):
    if len(points) <= 2: return len(points)

    res = 0
    for i in range(len(points)):
        slopes = defaultdict(int)
        for j in range(i + 1, len(points)):
            dx, dy = points[j][0] - points[i][0], points[j][1] - points[i][1]
            if dx == 0:
                slope = float('inf')
            else:
                slope = dy / dx
            slopes[slope] += 1
        res = max(res, max(slopes.values(), default=0) + 1)
    return res
```
