# Symmetric Tree

**Difficulty**: Easy  
**Tags**: Tree, Depth-First Search, Breadth-First Search, Binary Tree

## Question
Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

## Example Input/Output
**Input**: root = [1,2,2,3,4,4,3]  
**Output**: true  
**Explanation**:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
The tree is symmetric around its center.

**Input**: root = [1,2,2,null,3,null,3]  
**Output**: false  
**Explanation**:
```
    1
   / \
  2   2
   \   \
   3    3
```
The tree is not symmetric.

## Solution Algorithm
We can solve this problem using either a recursive or an iterative approach:

### Recursive Approach
1. Define a helper function that takes two nodes (left and right subtrees)
2. The tree is symmetric if:
   - Both nodes are null (base case)
   - Both nodes have the same value
   - The left subtree of the left node is a mirror of the right subtree of the right node
   - The right subtree of the left node is a mirror of the left subtree of the right node
3. Start by calling the helper function with the left and right children of the root

### Iterative Approach
1. Use a queue to perform a level-order traversal
2. Add pairs of nodes to the queue (left and right subtrees)
3. For each pair, check if they are mirrors of each other
4. If they are, add their children to the queue in mirrored order
5. Continue until the queue is empty or we find a non-symmetric pair

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

// Recursive approach
public boolean isSymmetric(TreeNode root) {
    if (root == null) {
        return true;
    }
    
    return isMirror(root.left, root.right);
}

private boolean isMirror(TreeNode left, TreeNode right) {
    // If both nodes are null, they are mirrors
    if (left == null && right == null) {
        return true;
    }
    
    // If only one node is null, they are not mirrors
    if (left == null || right == null) {
        return false;
    }
    
    // Check if the values are the same and the subtrees are mirrors
    return (left.val == right.val) 
        && isMirror(left.left, right.right) 
        && isMirror(left.right, right.left);
}
```

```java
// Iterative approach using a queue
public boolean isSymmetric(TreeNode root) {
    if (root == null) {
        return true;
    }
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root.left);
    queue.add(root.right);
    
    while (!queue.isEmpty()) {
        TreeNode node1 = queue.poll();
        TreeNode node2 = queue.poll();
        
        // If both nodes are null, continue
        if (node1 == null && node2 == null) {
            continue;
        }
        
        // If only one node is null or values are different, return false
        if (node1 == null || node2 == null || node1.val != node2.val) {
            return false;
        }
        
        // Add the pairs of nodes to check in mirrored order
        queue.add(node1.left);
        queue.add(node2.right);
        queue.add(node1.right);
        queue.add(node2.left);
    }
    
    return true;
}
``` 