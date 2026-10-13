# LeetCode 111: Minimum Depth of Binary Tree

## Problem Description

Given a binary tree, find its minimum depth.

The **minimum depth** is the number of nodes along the shortest path from the root node down to the nearest leaf node.

> **Note:** A leaf is a node with no children.

---

### Example 1:
```text
      3
     / \
    9   20
       /  \
      15   7
```
- **Input:** `root = [3,9,20,null,null,15,7]`
- **Output:** `2`
- **Explanation:** The path $3 \to 9$ has depth 2.

### Example 2:
```text
  2
   \
    3
     \
      4
       \
        5
         \
          6
```
- **Input:** `root = [2,null,3,null,4,null,5,null,6]`
- **Output:** `5`

---

## Important Edge Case to Keep in Mind

A common pitfall is writing:
$$\text{minDepth}(node) = 1 + \min(\text{minDepth}(left), \text{minDepth}(right))$$

This fails when one of the subtrees is empty (i.e., `null`). By definition, a path must end at a **leaf node** (a node with no children). If a node has only one child, the path must continue down that child's branch.

---

## Approach 1: Breadth-First Search (BFS / Level Order) — *Optimal for Minimum Depth*

### Intuition
BFS traverses the tree level by level. The first time we encounter a leaf node (`left == null && right == null`), we are guaranteed to be at the minimum depth. This avoids traversing the rest of the tree.

### Java Implementation

```java
import java.util.LinkedList;
import java.util.Queue;

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class SolutionBFS {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 1;

        while (!queue.isEmpty()) {
            int levelSize = queue.size();

            for (int i = 0; i < levelSize; i++) {
                TreeNode current = queue.poll();

                // If leaf node reached, return depth immediately
                if (current.left == null && current.right == null) {
                    return depth;
                }

                if (current.left != null) {
                    queue.offer(current.left);
                }
                if (current.right != null) {
                    queue.offer(current.right);
                }
            }
            depth++;
        }

        return depth;
    }
}
```

### Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$ in the worst case (e.g., when the tree is a skewed list or full tree with minimum depth at the bottom), but often much faster $\mathcal{O}(2^d)$ where $d$ is the minimum depth.
- **Space Complexity:** $\mathcal{O}(W)$ where $W$ is the maximum width of the tree (at most $\mathcal{O}(N)$).

---

## Approach 2: Depth-First Search (DFS / Recursion)

### Intuition
Traverse recursively. If a node has only one child, the path must go through that child. If both children exist, take the minimum of both subtrees.

### Java Implementation

```java
public class SolutionDFS {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        // If left subtree is null, recurse on right subtree
        if (root.left == null) {
            return 1 + minDepth(root.right);
        }

        // If right subtree is null, recurse on left subtree
        if (root.right == null) {
            return 1 + minDepth(root.left);
        }

        // If both children exist, pick the shorter path
        return 1 + Math.min(minDepth(root.left), minDepth(root.right));
    }
}
```

### Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$ because we may visit all nodes in the tree.
- **Space Complexity:** $\mathcal{O}(H)$ where $H$ is the height of the tree, representing the recursion call stack ($\mathcal{O}(N)$ in the worst case for a completely unbalanced tree, $\mathcal{O}(\log N)$ for a balanced tree).

---

## Comparison Summary

| Method | Best Used When | Time Complexity | Space Complexity |
|---|---|---|---|
| **BFS (Queue)** | The tree is large, and the shortest path is expected to be shallow. Exits early as soon as the first leaf is reached. | $\mathcal{O}(N)$ | $\mathcal{O}(W)$ |
| **DFS (Recursion)** | Concise code, but must explore full subtrees even when a leaf was already found at an earlier depth. | $\mathcal{O}(N)$ | $\mathcal{O}(H)$ |