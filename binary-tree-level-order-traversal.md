# Binary Tree Level Order Traversal

**Difficulty**: Medium  
**Tags**: Tree, Breadth-First Search, Binary Tree

## Question
Given the root of a binary tree, return the level order traversal of its nodes' values (i.e., from left to right, level by level).

## Example Input/Output
**Input**: root = [3,9,20,null,null,15,7]  
**Output**: [[3],[9,20],[15,7]]  
**Explanation**:
```
    3
   / \
  9  20
    /  \
   15   7
```

## Solution Algorithm
1. Use a breadth-first search (BFS) approach with a queue
2. Start by adding the root node to the queue
3. For each level:
   - Determine the number of nodes at the current level (queue size)
   - Process all nodes at the current level and add their children to the queue
   - Add the values of the current level nodes to the result
4. Return the level order traversal

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
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    
    if (root == null) {
        return result;
    }
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> currentLevel = new ArrayList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            currentLevel.add(node.val);
            
            if (node.left != null) {
                queue.offer(node.left);
            }
            
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        
        result.add(currentLevel);
    }
    
    return result;
}
``` 