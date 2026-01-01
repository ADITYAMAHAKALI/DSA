> [!note] Description
> DP on Trees involves solving problems where data is hierarchical. Usually involves post-order traversal (solving for children first, then combining results for the parent).

## 1. House Robber III

### Intuition
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. The police will be contacted if two directly-linked houses were broken into on the same night.
Return the maximum amount of money the thief can rob.
For each node, we have two states:
1.  **Rob this node:** Value = `node.val` + `rob(grandchildren)`. (Cannot rob children).
2.  **Skip this node:** Value = `rob(children)`. (Can rob children, but don't have to).

### Top-Down (Memoization)
```python
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        memo = {}

        def dp(node):
            if not node:
                return 0
            if node in memo:
                return memo[node]

            # Option 1: Rob root
            val1 = node.val
            if node.left:
                val1 += dp(node.left.left) + dp(node.left.right)
            if node.right:
                val1 += dp(node.right.left) + dp(node.right.right)

            # Option 2: Skip root
            val2 = dp(node.left) + dp(node.right)

            memo[node] = max(val1, val2)
            return memo[node]

        return dp(root)
```

### Bottom-Up (Optimized DFS)
Instead of full memoization which looks up grandchildren, we can just return two values for every node: `[rob_money, skip_money]`.
`rob_money = node.val + left.skip + right.skip`
`skip_money = max(left) + max(right)` where `max(child)` is `max(child.rob, child.skip)`.

```python
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:

        def helper(node):
            if not node:
                return (0, 0) # (rob this node, skip this node)

            left = helper(node.left)
            right = helper(node.right)

            # If we rob this node, we cannot rob children
            rob_now = node.val + left[1] + right[1]

            # If we skip this node, we can choose to rob or skip children
            skip_now = max(left) + max(right)

            return (rob_now, skip_now)

        return max(helper(root))
```

### Complexity
- **Time:** $O(N)$
- **Space:** $O(H)$ (recursion stack)

---

## 2. Binary Tree Maximum Path Sum

### Intuition
A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.
For any node `u`, the max path passing through `u` (as the highest node in the path) is `max_gain(left_child) + u.val + max_gain(right_child)`.
The helper function `max_gain` returns the max path sum starting at `u` and going down one branch.

### Top-Down (DFS with Global Max)
```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.max_sum = float('-inf')

        def gain_from_subtree(node):
            if not node:
                return 0

            # We ignore paths with negative sums (better to not include them)
            left_gain = max(gain_from_subtree(node.left), 0)
            right_gain = max(gain_from_subtree(node.right), 0)

            # Price to start a new path where `node` is the highest point
            price_newpath = node.val + left_gain + right_gain

            self.max_sum = max(self.max_sum, price_newpath)

            # For recursion return the max gain if we continue the same path
            return node.val + max(left_gain, right_gain)

        gain_from_subtree(root)
        return self.max_sum
```

### Complexity
- **Time:** $O(N)$
- **Space:** $O(H)$

---

## 3. Diameter of Binary Tree

### Intuition
Given the root of a binary tree, return the length of the **diameter** of the tree. The diameter is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.
Similar to max path sum, but we count edges (or nodes - 1). For a node, the longest path passing through it is `depth(left) + depth(right)`.

### Top-Down (DFS with Global Max)
```python
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.diameter = 0

        def depth(node):
            if not node:
                return 0

            left = depth(node.left)
            right = depth(node.right)

            # The path through this node uses left depth and right depth
            self.diameter = max(self.diameter, left + right)

            # Return depth of this node
            return 1 + max(left, right)

        depth(root)
        return self.diameter
```

### Complexity
- **Time:** $O(N)$
- **Space:** $O(H)$
