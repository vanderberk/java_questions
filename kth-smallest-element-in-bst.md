# Kth Smallest Element in a BST

**Difficulty**: Medium  
**Tags**: Tree, Depth-First Search, Binary Search Tree, Binary Tree

## Question
Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

## Example Input/Output
**Input**: root = [3,1,4,null,2], k = 1  
**Output**: 1

**Input**: root = [5,3,6,2,4,null,null,1], k = 3  
**Output**: 3

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Inorder Traversal (Recursive)
1. Perform an inorder traversal of the BST, which visits nodes in ascending order.
2. Keep track of the number of nodes visited.
3. When the count equals k, return the value of the current node.

### Approach 2: Inorder Traversal (Iterative)
1. Use a stack to perform an iterative inorder traversal.
2. Keep track of the number of nodes visited.
3. When the count equals k, return the value of the current node.

### Approach 3: Follow-up Optimization
If the BST is modified often and the kth smallest element is queried frequently, we can augment the BST by storing the count of nodes in the left subtree for each node. This allows us to find the kth smallest element in O(h) time, where h is the height of the tree.

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
// Approach 1: Inorder Traversal (Recursive)
class Solution {
    private int count = 0;
    private int result = 0;
    
    public int kthSmallest(TreeNode root, int k) {
        inorderTraversal(root, k);
        return result;
    }
    
    private void inorderTraversal(TreeNode node, int k) {
        if (node == null) {
            return;
        }
        
        // Traverse left subtree
        inorderTraversal(node.left, k);
        
        // Process current node
        count++;
        if (count == k) {
            result = node.val;
            return;
        }
        
        // Traverse right subtree
        inorderTraversal(node.right, k);
    }
}
```

```java
// Approach 2: Inorder Traversal (Iterative)
public int kthSmallest(TreeNode root, int k) {
    Stack<TreeNode> stack = new Stack<>();
    TreeNode current = root;
    int count = 0;
    
    while (current != null || !stack.isEmpty()) {
        // Traverse to the leftmost node
        while (current != null) {
            stack.push(current);
            current = current.left;
        }
        
        // Process the current node
        current = stack.pop();
        count++;
        
        if (count == k) {
            return current.val;
        }
        
        // Move to the right subtree
        current = current.right;
    }
    
    return -1; // This should not happen if k is valid
}
```

```java
// Approach 3: Follow-up Optimization with Augmented BST
class AugmentedTreeNode {
    int val;
    int leftCount; // Count of nodes in the left subtree
    AugmentedTreeNode left;
    AugmentedTreeNode right;
    
    public AugmentedTreeNode(int val) {
        this.val = val;
        this.leftCount = 0;
        this.left = null;
        this.right = null;
    }
}

public int kthSmallest(TreeNode root, int k) {
    // Convert the BST to an augmented BST
    AugmentedTreeNode augmentedRoot = buildAugmentedBST(root);
    
    // Find the kth smallest element
    return findKthSmallest(augmentedRoot, k);
}

private AugmentedTreeNode buildAugmentedBST(TreeNode node) {
    if (node == null) {
        return null;
    }
    
    AugmentedTreeNode augmentedNode = new AugmentedTreeNode(node.val);
    augmentedNode.left = buildAugmentedBST(node.left);
    augmentedNode.right = buildAugmentedBST(node.right);
    
    // Calculate the count of nodes in the left subtree
    if (augmentedNode.left != null) {
        augmentedNode.leftCount = getSize(augmentedNode.left);
    }
    
    return augmentedNode;
}

private int getSize(AugmentedTreeNode node) {
    if (node == null) {
        return 0;
    }
    
    return 1 + getSize(node.left) + getSize(node.right);
}

private int findKthSmallest(AugmentedTreeNode node, int k) {
    if (node == null) {
        return -1;
    }
    
    // If the kth smallest is in the left subtree
    if (k <= node.leftCount) {
        return findKthSmallest(node.left, k);
    }
    
    // If the kth smallest is the current node
    if (k == node.leftCount + 1) {
        return node.val;
    }
    
    // If the kth smallest is in the right subtree
    return findKthSmallest(node.right, k - node.leftCount - 1);
}
``` 