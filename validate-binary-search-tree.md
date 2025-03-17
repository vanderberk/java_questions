# Validate Binary Search Tree

**Difficulty**: Medium  
**Tags**: Tree, Depth-First Search, Binary Search Tree, Binary Tree

## Question
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

## Example Input/Output
**Input**: root = [2,1,3]  
**Output**: true  
**Explanation**:
```
    2
   / \
  1   3
```
This is a valid BST.

**Input**: root = [5,1,4,null,null,3,6]  
**Output**: false  
**Explanation**:
```
    5
   / \
  1   4
     / \
    3   6
```
The value of the root's right child is 4, but it should be greater than 5.

## Solution Algorithm
We can solve this problem using several approaches:

### Approach 1: Recursive with Range Checking
1. Create a recursive function that takes a node, a minimum value, and a maximum value.
2. For each node, check if its value is within the valid range (min < node.val < max).
3. Recursively check the left subtree with an updated maximum value (the current node's value).
4. Recursively check the right subtree with an updated minimum value (the current node's value).
5. If any check fails, return false. If all checks pass, return true.

### Approach 2: In-order Traversal
1. Perform an in-order traversal of the tree.
2. In a valid BST, the in-order traversal should yield values in ascending order.
3. Keep track of the previous value during the traversal.
4. If at any point the current value is less than or equal to the previous value, return false.
5. If the traversal completes without any issues, return true.

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

// Approach 1: Recursive with Range Checking
public boolean isValidBST(TreeNode root) {
    return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

private boolean isValidBST(TreeNode node, long min, long max) {
    // Base case: empty tree is a valid BST
    if (node == null) {
        return true;
    }
    
    // Check if the current node's value is within the valid range
    if (node.val <= min || node.val >= max) {
        return false;
    }
    
    // Recursively check the left and right subtrees
    return isValidBST(node.left, min, node.val) && isValidBST(node.right, node.val, max);
}
```

```java
// Approach 2: In-order Traversal
public boolean isValidBST(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    TreeNode prev = null;
    
    while (!stack.isEmpty() || root != null) {
        // Traverse to the leftmost node
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
        
        // Process the current node
        root = stack.pop();
        
        // Check if the current value is greater than the previous value
        if (prev != null && root.val <= prev.val) {
            return false;
        }
        
        // Update the previous value
        prev = root;
        
        // Move to the right subtree
        root = root.right;
    }
    
    return true;
}
```

```java
// Approach 3: Recursive In-order Traversal
public boolean isValidBST(TreeNode root) {
    List<Integer> values = new ArrayList<>();
    inOrderTraversal(root, values);
    
    // Check if the values are in ascending order
    for (int i = 1; i < values.size(); i++) {
        if (values.get(i) <= values.get(i - 1)) {
            return false;
        }
    }
    
    return true;
}

private void inOrderTraversal(TreeNode node, List<Integer> values) {
    if (node == null) {
        return;
    }
    
    inOrderTraversal(node.left, values);
    values.add(node.val);
    inOrderTraversal(node.right, values);
}
``` 