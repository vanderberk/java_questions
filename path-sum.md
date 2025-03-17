# Path Sum

**Difficulty**: Easy  
**Tags**: Tree, Depth-First Search, Binary Tree, Recursion

## Question
Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a root-to-leaf path such that adding up all the values along the path equals `targetSum`.

A leaf is a node with no children.

## Example Input/Output
**Input**: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22  
**Output**: true  
**Explanation**: The root-to-leaf path with the target sum is 5 -> 4 -> 11 -> 2.

```
      5
     / \
    4   8
   /   / \
  11  13  4
 / \      \
7   2      1
```

**Input**: root = [1,2,3], targetSum = 5  
**Output**: false  
**Explanation**: There are two root-to-leaf paths in the tree:
(1 -> 2): Sum = 3
(1 -> 3): Sum = 4
There is no root-to-leaf path with sum = 5.

## Solution Algorithm
We can solve this problem using a depth-first search (DFS) approach:

1. Start from the root and subtract the node's value from the target sum.
2. If we reach a leaf node and the remaining sum equals the leaf's value, return true.
3. Otherwise, recursively check the left and right subtrees with the updated sum.
4. If both left and right subtrees return false, return false.

We can implement this using either recursion or an iterative approach with a stack.

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
public class Solution {
    // Recursive approach
    public boolean hasPathSum(TreeNode root, int targetSum) {
        // Base case: empty tree
        if (root == null) {
            return false;
        }
        
        // Check if it's a leaf node and the value equals the remaining sum
        if (root.left == null && root.right == null) {
            return root.val == targetSum;
        }
        
        // Recursively check left and right subtrees with updated sum
        return hasPathSum(root.left, targetSum - root.val) || 
               hasPathSum(root.right, targetSum - root.val);
    }
}
```

```java
// Iterative approach using a stack
public boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) {
        return false;
    }
    
    // Stack to store nodes and their corresponding remaining sums
    Stack<TreeNode> nodeStack = new Stack<>();
    Stack<Integer> sumStack = new Stack<>();
    
    nodeStack.push(root);
    sumStack.push(targetSum);
    
    while (!nodeStack.isEmpty()) {
        TreeNode currentNode = nodeStack.pop();
        int currentSum = sumStack.pop();
        
        // Check if it's a leaf node and the value equals the remaining sum
        if (currentNode.left == null && currentNode.right == null && 
            currentNode.val == currentSum) {
            return true;
        }
        
        // Push right child to stack
        if (currentNode.right != null) {
            nodeStack.push(currentNode.right);
            sumStack.push(currentSum - currentNode.val);
        }
        
        // Push left child to stack
        if (currentNode.left != null) {
            nodeStack.push(currentNode.left);
            sumStack.push(currentSum - currentNode.val);
        }
    }
    
    return false;
}
```

```java
// Alternative recursive approach with a helper method
public boolean hasPathSum(TreeNode root, int targetSum) {
    return dfs(root, 0, targetSum);
}

private boolean dfs(TreeNode node, int currentSum, int targetSum) {
    if (node == null) {
        return false;
    }
    
    currentSum += node.val;
    
    // Check if it's a leaf node and the sum equals the target
    if (node.left == null && node.right == null) {
        return currentSum == targetSum;
    }
    
    // Recursively check left and right subtrees
    return dfs(node.left, currentSum, targetSum) || 
           dfs(node.right, currentSum, targetSum);
}
``` 