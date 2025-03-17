# Serialize and Deserialize N-ary Tree

**Difficulty**: Hard  
**Tags**: Tree, Depth-First Search, Breadth-First Search, String, Design

## Question
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize an N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that an N-ary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following `3-ary` tree:

```
    1
   /|\
  3 2 4
 / \
5   6
```

as `[1,null,3,2,4,null,5,6]`

You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

## Example Input/Output
**Input**: root = [1,null,3,2,4,null,5,6]  
**Output**: [1,null,3,2,4,null,5,6]  
**Explanation**: The input and output are the same serialized representation of the tree.

**Input**: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]  
**Output**: [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]

## Solution Algorithm
There are several approaches to serialize and deserialize an N-ary tree:

### Approach 1: Level Order Traversal (BFS)
1. **Serialization**:
   - Use a queue to perform a level-order traversal of the tree.
   - For each node, append its value to the result string.
   - After processing a node, append a special marker (e.g., "null") to indicate the end of its children.
   - Continue until the queue is empty.

2. **Deserialization**:
   - Parse the serialized string to get the values.
   - Use a queue to build the tree level by level.
   - For each node, read values until encountering the special marker, and add them as children.
   - Continue until all values are processed.

### Approach 2: Depth-First Search (DFS)
1. **Serialization**:
   - Use a preorder traversal (root, then children).
   - For each node, append its value to the result string.
   - After processing all children of a node, append a special marker.
   - Continue recursively for all nodes.

2. **Deserialization**:
   - Parse the serialized string to get the values.
   - Use recursion to build the tree in a preorder fashion.
   - For each value, create a node and recursively build its children until encountering the special marker.

## Solution Code
```java
// Definition for a Node
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
        children = new ArrayList<>();
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
}

// Approach 1: Level Order Traversal (BFS)
class Codec {
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if (root == null) {
            return "";
        }
        
        StringBuilder sb = new StringBuilder();
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            Node node = queue.poll();
            
            if (node == null) {
                sb.append("null,");
            } else {
                sb.append(node.val).append(",");
                
                // Add a null marker after all children
                for (Node child : node.children) {
                    queue.offer(child);
                }
                queue.offer(null); // Marker for end of children
            }
        }
        
        return sb.toString();
    }
    
    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if (data == null || data.isEmpty()) {
            return null;
        }
        
        String[] values = data.split(",");
        Queue<Node> queue = new LinkedList<>();
        Node root = new Node(Integer.parseInt(values[0]), new ArrayList<>());
        queue.offer(root);
        
        int i = 1;
        while (!queue.isEmpty() && i < values.length) {
            Node parent = queue.poll();
            
            // Read children until null marker
            while (i < values.length && !values[i].equals("null")) {
                Node child = new Node(Integer.parseInt(values[i]), new ArrayList<>());
                parent.children.add(child);
                queue.offer(child);
                i++;
            }
            
            i++; // Skip the null marker
        }
        
        return root;
    }
}
```

```java
// Approach 2: Depth-First Search (DFS)
class Codec {
    private static final String NULL_MARKER = "#";
    private static final String SEPARATOR = ",";
    
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }
    
    private void serializeHelper(Node node, StringBuilder sb) {
        if (node == null) {
            sb.append(NULL_MARKER).append(SEPARATOR);
            return;
        }
        
        sb.append(node.val).append(SEPARATOR);
        sb.append(node.children.size()).append(SEPARATOR);
        
        for (Node child : node.children) {
            serializeHelper(child, sb);
        }
    }
    
    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if (data == null || data.isEmpty()) {
            return null;
        }
        
        Queue<String> queue = new LinkedList<>(Arrays.asList(data.split(SEPARATOR)));
        return deserializeHelper(queue);
    }
    
    private Node deserializeHelper(Queue<String> queue) {
        String val = queue.poll();
        
        if (val.equals(NULL_MARKER)) {
            return null;
        }
        
        Node root = new Node(Integer.parseInt(val), new ArrayList<>());
        int size = Integer.parseInt(queue.poll());
        
        for (int i = 0; i < size; i++) {
            root.children.add(deserializeHelper(queue));
        }
        
        return root;
    }
}
```

```java
// Alternative approach using JSON-like format
class Codec {
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if (root == null) {
            return "null";
        }
        
        StringBuilder sb = new StringBuilder();
        sb.append("{").append(root.val).append(":[");
        
        for (int i = 0; i < root.children.size(); i++) {
            sb.append(serialize(root.children.get(i)));
            if (i < root.children.size() - 1) {
                sb.append(",");
            }
        }
        
        sb.append("]}");
        return sb.toString();
    }
    
    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if (data.equals("null")) {
            return null;
        }
        
        // Parse the value
        int valueEnd = data.indexOf(":");
        int value = Integer.parseInt(data.substring(1, valueEnd));
        Node root = new Node(value, new ArrayList<>());
        
        // Check if there are children
        if (data.charAt(valueEnd + 1) == '[' && data.charAt(valueEnd + 2) != ']') {
            // Parse the children
            String childrenStr = data.substring(valueEnd + 2, data.length() - 2);
            List<String> childrenSerialized = splitChildren(childrenStr);
            
            for (String childStr : childrenSerialized) {
                root.children.add(deserialize(childStr));
            }
        }
        
        return root;
    }
    
    private List<String> splitChildren(String s) {
        List<String> result = new ArrayList<>();
        int bracketCount = 0;
        int start = 0;
        
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            
            if (c == '{') {
                bracketCount++;
            } else if (c == '}') {
                bracketCount--;
            } else if (c == ',' && bracketCount == 0) {
                result.add(s.substring(start, i));
                start = i + 1;
            }
        }
        
        result.add(s.substring(start));
        return result;
    }
}
``` 