# Convert Sorted Array to Binary Search Tree

**Difficulty**: Easy  
**Tags**: Array, Divide and Conquer, Tree, Binary Search Tree, Binary Tree

## Question
Given an integer array `nums` where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

## Example Input/Output
**Input**: nums = [-10,-3,0,5,9]  
**Output**: [0,-3,9,-10,null,5]  
**Explanation**: The output represents the following height-balanced BST:
```
      0
     / \
   -3   9
   /   /
 -10  5
```

**Input**: nums = [1,3]  
**Output**: [3,1]  
**Explanation**: The output represents the following height-balanced BST:
```
  3
 /
1
```

## Solution Algorithm
We can solve this problem using a recursive approach:

1. Choose the middle element of the array as the root of the BST.
2. Recursively construct the left subtree using the elements to the left of the middle element.
3. Recursively construct the right subtree using the elements to the right of the middle element.
4. Return the root.

This approach ensures that the tree is height-balanced because we always choose the middle element as the root, which divides the array into two roughly equal parts.

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
public TreeNode sortedArrayToBST(int[] nums) {
    if (nums == null || nums.length == 0) {
        return null;
    }
    
    return constructBST(nums, 0, nums.length - 1);
}

private TreeNode constructBST(int[] nums, int left, int right) {
    if (left > right) {
        return null;
    }
    
    // Choose the middle element as the root
    int mid = left + (right - left) / 2;
    TreeNode root = new TreeNode(nums[mid]);
    
    // Recursively construct the left and right subtrees
    root.left = constructBST(nums, left, mid - 1);
    root.right = constructBST(nums, mid + 1, right);
    
    return root;
}
```

```java
// Alternative implementation with array slicing (less efficient but more readable)
public TreeNode sortedArrayToBST(int[] nums) {
    if (nums == null || nums.length == 0) {
        return null;
    }
    
    int mid = nums.length / 2;
    TreeNode root = new TreeNode(nums[mid]);
    
    // Create left and right subarrays
    int[] leftArray = Arrays.copyOfRange(nums, 0, mid);
    int[] rightArray = Arrays.copyOfRange(nums, mid + 1, nums.length);
    
    // Recursively construct the left and right subtrees
    root.left = sortedArrayToBST(leftArray);
    root.right = sortedArrayToBST(rightArray);
    
    return root;
}
``` 