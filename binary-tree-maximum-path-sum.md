# Binary Tree Maximum Path Sum

**Difficulty**: Hard  
**Tags**: Tree, Depth-First Search, Dynamic Programming, Binary Tree

## Question
A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return the maximum **path sum** of any non-empty path.

## Example Input/Output
**Input**: root = [1,2,3]  
**Output**: 6  
**Explanation**: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

**Input**: root = [-10,9,20,null,null,15,7]  
**Output**: 42  
**Explanation**: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

## Solution Algorithm
This problem can be solved using a recursive depth-first search (DFS) approach:

1. For each node, we need to calculate two values:
   - The maximum path sum that includes the node and one of its subtrees (used for extending the path upwards).
   - The maximum path sum that includes the node and possibly both of its subtrees (used for updating the global maximum).

2. For a given node:
   - Calculate the maximum path sum for the left subtree.
   - Calculate the maximum path sum for the right subtree.
   - The maximum path sum that includes the node and one of its subtrees is: `node.val + max(leftSum, rightSum, 0)`.
   - The maximum path sum that includes the node and possibly both of its subtrees is: `node.val + max(0, leftSum) + max(0, rightSum)`.

3. Update the global maximum with the maximum path sum that includes the node and possibly both of its subtrees.

4. Return the maximum path sum that includes the node and one of its subtrees for the recursive calls.

## Solution Code
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public int maxPathSum(TreeNode root) {
    int[] maxSum = new int[1];
    maxSum[0] = Integer.MIN_VALUE;
    
    maxPathSumHelper(root, maxSum);
    
    return maxSum[0];
}

private int maxPathSumHelper(TreeNode node, int[] maxSum) {
    if (node == null) {
        return 0;
    }
    
    // Calculate the maximum path sum for the left and right subtrees
    int leftSum = Math.max(0, maxPathSumHelper(node.left, maxSum));
    int rightSum = Math.max(0, maxPathSumHelper(node.right, maxSum));
    
    // Update the global maximum with the path sum that includes the current node
    // and possibly both of its subtrees
    maxSum[0] = Math.max(maxSum[0], node.val + leftSum + rightSum);
    
    // Return the maximum path sum that includes the current node and one of its subtrees
    // (used for extending the path upwards)
    return node.val + Math.max(leftSum, rightSum);
}
```

```java
// Alternative implementation with more explicit variable names
public int maxPathSum(TreeNode root) {
    MaxPathSumResult result = new MaxPathSumResult();
    calculateMaxPathSum(root, result);
    return result.maxPathSum;
}

private int calculateMaxPathSum(TreeNode node, MaxPathSumResult result) {
    if (node == null) {
        return 0;
    }
    
    // Calculate the maximum path sum for the left and right subtrees
    int leftMaxSum = Math.max(0, calculateMaxPathSum(node.left, result));
    int rightMaxSum = Math.max(0, calculateMaxPathSum(node.right, result));
    
    // Calculate the maximum path sum that includes the current node
    // and possibly both of its subtrees
    int currentNodeMaxPathSum = node.val + leftMaxSum + rightMaxSum;
    
    // Update the global maximum
    result.maxPathSum = Math.max(result.maxPathSum, currentNodeMaxPathSum);
    
    // Return the maximum path sum that includes the current node and one of its subtrees
    // (used for extending the path upwards)
    return node.val + Math.max(leftMaxSum, rightMaxSum);
}

class MaxPathSumResult {
    int maxPathSum = Integer.MIN_VALUE;
}
``` 