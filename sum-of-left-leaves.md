# Sum of Left Leaves

**Difficulty**: Easy  
**Tags**: Tree, Depth-First Search, Breadth-First Search, Binary Tree

## Question
Given the `root` of a binary tree, return the sum of all left leaves.

A leaf is a node with no children. A left leaf is a leaf that is the left child of another node.

## Example Input/Output
**Input**: root = [3,9,20,null,null,15,7]  
**Output**: 24  
**Explanation**: There are two left leaves in the binary tree, with values 9 and 15. The sum is 24.
```
    3
   / \
  9  20
    /  \
   15   7
```

**Input**: root = [1]  
**Output**: 0  
**Explanation**: There are no left leaves in the binary tree, so the sum is 0.

## Solution Algorithm
We can solve this problem using several approaches:

### Approach 1: Recursive DFS
1. If the root is null, return 0.
2. Check if the left child is a leaf node (no children). If it is, add its value to the sum.
3. Recursively calculate the sum of left leaves in the left and right subtrees.
4. Return the total sum.

### Approach 2: Iterative DFS (Stack)
1. Use a stack to perform depth-first traversal.
2. For each node, check if its left child is a leaf node. If it is, add its value to the sum.
3. Push the right child and then the left child onto the stack (if they exist).
4. Continue until the stack is empty.
5. Return the total sum.

### Approach 3: Iterative BFS (Queue)
1. Use a queue to perform breadth-first traversal.
2. For each node, check if its left child is a leaf node. If it is, add its value to the sum.
3. Add the left and right children to the queue (if they exist).
4. Continue until the queue is empty.
5. Return the total sum.

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
// Approach 1: Recursive DFS
public int sumOfLeftLeaves(TreeNode root) {
    if (root == null) {
        return 0;
    }
    
    int sum = 0;
    
    // Check if the left child is a leaf node
    if (root.left != null && root.left.left == null && root.left.right == null) {
        sum += root.left.val;
    }
    
    // Recursively calculate the sum for left and right subtrees
    sum += sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
    
    return sum;
}
```

```java
// Approach 2: Iterative DFS (Stack)
public int sumOfLeftLeaves(TreeNode root) {
    if (root == null) {
        return 0;
    }
    
    int sum = 0;
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        
        // Check if the left child is a leaf node
        if (node.left != null && node.left.left == null && node.left.right == null) {
            sum += node.left.val;
        }
        
        // Push right child first (will be processed after left)
        if (node.right != null) {
            stack.push(node.right);
        }
        
        // Push left child
        if (node.left != null) {
            stack.push(node.left);
        }
    }
    
    return sum;
}
```

```java
// Approach 3: Iterative BFS (Queue)
public int sumOfLeftLeaves(TreeNode root) {
    if (root == null) {
        return 0;
    }
    
    int sum = 0;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll();
        
        // Check if the left child is a leaf node
        if (node.left != null && node.left.left == null && node.left.right == null) {
            sum += node.left.val;
        }
        
        // Add left and right children to the queue
        if (node.left != null) {
            queue.offer(node.left);
        }
        
        if (node.right != null) {
            queue.offer(node.right);
        }
    }
    
    return sum;
}
``` 