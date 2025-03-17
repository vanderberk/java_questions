# Balanced Binary Tree

**Difficulty**: Easy  
**Tags**: Tree, Depth-First Search, Binary Tree, Recursion

## Question
Given a binary tree, determine if it is height-balanced.

A height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differs by more than 1.

## Example Input/Output
**Input**: root = [3,9,20,null,null,15,7]  
**Output**: true  
**Explanation**: The tree looks like:
```
    3
   / \
  9  20
    /  \
   15   7
```
The depth of the left subtree (with root 9) is 1.
The depth of the right subtree (with root 20) is 2.
The difference is 1, which is not more than 1, so the tree is balanced.

**Input**: root = [1,2,2,3,3,null,null,4,4]  
**Output**: false  
**Explanation**: The tree looks like:
```
        1
       / \
      2   2
     / \
    3   3
   / \
  4   4
```
The depth of the left subtree (with root 2) is 3.
The depth of the right subtree (with root 2) is 1.
The difference is 2, which is more than 1, so the tree is not balanced.

## Solution Algorithm
We can solve this problem using a recursive approach:

1. Define a helper function to calculate the height of a tree:
   - If the node is null, return 0.
   - Recursively calculate the height of the left and right subtrees.
   - Return 1 + max(left height, right height).

2. For each node in the tree:
   - Check if the left subtree is balanced.
   - Check if the right subtree is balanced.
   - Check if the difference between the heights of the left and right subtrees is at most 1.
   - If all conditions are met, the tree is balanced.

3. We can optimize this by combining the height calculation and balance check in a single traversal:
   - If a subtree is unbalanced, return -1 to indicate it.
   - Otherwise, return the actual height of the subtree.

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
    // Top-down approach (less efficient)
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        // Check if the current node is balanced
        int leftHeight = getHeight(root.left);
        int rightHeight = getHeight(root.right);
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return false;
        }
        
        // Recursively check if left and right subtrees are balanced
        return isBalanced(root.left) && isBalanced(root.right);
    }
    
    private int getHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        return 1 + Math.max(getHeight(node.left), getHeight(node.right));
    }
}
```

```java
// Bottom-up approach (more efficient)
public boolean isBalanced(TreeNode root) {
    return checkHeight(root) != -1;
}

private int checkHeight(TreeNode node) {
    if (node == null) {
        return 0;
    }
    
    // Check left subtree
    int leftHeight = checkHeight(node.left);
    if (leftHeight == -1) {
        return -1; // Left subtree is unbalanced
    }
    
    // Check right subtree
    int rightHeight = checkHeight(node.right);
    if (rightHeight == -1) {
        return -1; // Right subtree is unbalanced
    }
    
    // Check if current node is balanced
    if (Math.abs(leftHeight - rightHeight) > 1) {
        return -1; // Current node is unbalanced
    }
    
    // Return the height of the current node
    return 1 + Math.max(leftHeight, rightHeight);
}
``` 