# Serialize and Deserialize Binary Tree

**Difficulty**: Hard  
**Tags**: Tree, Design, DFS, BFS, String

## Question
Design an algorithm to serialize and deserialize a binary tree. Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection. Deserialization is the reverse process, where the sequence is converted back into the original data structure.

## Example Input/Output
**Input**: root = [1,2,3,null,null,4,5]  
**Output**: [1,2,3,null,null,4,5]  
**Explanation**: The binary tree is:
```
    1
   / \
  2   3
     / \
    4   5
```

## Solution Algorithm
1. **Serialization**:
   - Use a preorder traversal (root, left, right) to serialize the tree
   - Use a special marker (e.g., "null") for null nodes
   - Join the values with a delimiter (e.g., ",")

2. **Deserialization**:
   - Split the serialized string by the delimiter
   - Use a queue to process the values in order
   - Recursively build the tree following the same preorder traversal

## Solution Code
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    private static final String NULL_MARKER = "null";
    private static final String DELIMITER = ",";

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }
    
    private void serializeHelper(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append(NULL_MARKER).append(DELIMITER);
            return;
        }
        
        // Preorder traversal: root, left, right
        sb.append(node.val).append(DELIMITER);
        serializeHelper(node.left, sb);
        serializeHelper(node.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        Queue<String> nodes = new LinkedList<>(Arrays.asList(data.split(DELIMITER)));
        return deserializeHelper(nodes);
    }
    
    private TreeNode deserializeHelper(Queue<String> nodes) {
        String val = nodes.poll();
        
        if (val == null || val.equals(NULL_MARKER)) {
            return null;
        }
        
        // Create the current node
        TreeNode node = new TreeNode(Integer.parseInt(val));
        
        // Recursively build left and right subtrees
        node.left = deserializeHelper(nodes);
        node.right = deserializeHelper(nodes);
        
        return node;
    }
} 