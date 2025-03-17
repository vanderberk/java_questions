# Alien Dictionary

**Difficulty**: Hard  
**Tags**: Graph, Topological Sort, String, Depth-First Search, Breadth-First Search

## Question
There is a new alien language that uses the English alphabet. However, the order of the letters is unknown to you.

You are given a list of strings `words` from the alien language's dictionary, where the strings in `words` are sorted lexicographically by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no solution, return "". If there are multiple solutions, return any of them.

A string `s` is lexicographically smaller than a string `t` if at the first letter where they differ, the letter in `s` comes before the letter in `t` in the alien language. If the first `min(s.length, t.length)` letters are the same, then `s` is smaller if and only if `s.length < t.length`.

## Example Input/Output
**Input**: words = ["wrt","wrf","er","ett","rftt"]  
**Output**: "wertf"

**Input**: words = ["z","x"]  
**Output**: "zx"

**Input**: words = ["z","x","z"]  
**Output**: ""  
**Explanation**: The order is invalid, so return "".

## Solution Algorithm
This problem can be solved using topological sorting:

1. Build a graph where each node is a letter and each edge represents the relative order between two letters.
2. For each adjacent pair of words, find the first differing character and add an edge from the character in the first word to the character in the second word.
3. Perform a topological sort on the graph to get the order of the letters.
4. If there is a cycle in the graph, return an empty string as the order is invalid.

### Approach 1: Using Depth-First Search (DFS)
1. Build the graph as described above.
2. Use DFS with three states for each node: unvisited, visiting, and visited.
3. If we encounter a node that is in the "visiting" state during DFS, there is a cycle.
4. Add nodes to the result in reverse order of their finishing time.

### Approach 2: Using Breadth-First Search (BFS) - Kahn's Algorithm
1. Build the graph and calculate the in-degree of each node.
2. Start with nodes that have an in-degree of 0.
3. For each node, decrement the in-degree of its neighbors.
4. If a neighbor's in-degree becomes 0, add it to the queue.
5. If we can't visit all nodes, there is a cycle.

## Solution Code
```java
// Approach 1: DFS
public String alienOrder(String[] words) {
    // Step 1: Build the graph
    Map<Character, Set<Character>> graph = new HashMap<>();
    Map<Character, Integer> visited = new HashMap<>();
    
    // Initialize the graph with all unique characters
    for (String word : words) {
        for (char c : word.toCharArray()) {
            graph.putIfAbsent(c, new HashSet<>());
            visited.putIfAbsent(c, 0); // 0: unvisited, 1: visiting, 2: visited
        }
    }
    
    // Add edges to the graph
    for (int i = 0; i < words.length - 1; i++) {
        String word1 = words[i];
        String word2 = words[i + 1];
        
        // Check if word2 is a prefix of word1
        if (word1.length() > word2.length() && word1.startsWith(word2)) {
            return "";
        }
        
        // Find the first differing character
        int minLength = Math.min(word1.length(), word2.length());
        for (int j = 0; j < minLength; j++) {
            char c1 = word1.charAt(j);
            char c2 = word2.charAt(j);
            
            if (c1 != c2) {
                graph.get(c1).add(c2);
                break;
            }
        }
    }
    
    // Step 2: Topological sort using DFS
    StringBuilder result = new StringBuilder();
    
    for (char c : graph.keySet()) {
        if (visited.get(c) == 0) {
            if (!dfs(c, graph, visited, result)) {
                return "";
            }
        }
    }
    
    return result.reverse().toString();
}

private boolean dfs(char c, Map<Character, Set<Character>> graph, Map<Character, Integer> visited, StringBuilder result) {
    visited.put(c, 1); // Mark as visiting
    
    for (char neighbor : graph.get(c)) {
        if (visited.get(neighbor) == 1) {
            return false; // Cycle detected
        }
        
        if (visited.get(neighbor) == 0) {
            if (!dfs(neighbor, graph, visited, result)) {
                return false;
            }
        }
    }
    
    visited.put(c, 2); // Mark as visited
    result.append(c);
    return true;
}
```

```java
// Approach 2: BFS (Kahn's Algorithm)
public String alienOrder(String[] words) {
    // Step 1: Build the graph
    Map<Character, Set<Character>> graph = new HashMap<>();
    Map<Character, Integer> inDegree = new HashMap<>();
    
    // Initialize the graph with all unique characters
    for (String word : words) {
        for (char c : word.toCharArray()) {
            graph.putIfAbsent(c, new HashSet<>());
            inDegree.putIfAbsent(c, 0);
        }
    }
    
    // Add edges to the graph
    for (int i = 0; i < words.length - 1; i++) {
        String word1 = words[i];
        String word2 = words[i + 1];
        
        // Check if word2 is a prefix of word1
        if (word1.length() > word2.length() && word1.startsWith(word2)) {
            return "";
        }
        
        // Find the first differing character
        int minLength = Math.min(word1.length(), word2.length());
        for (int j = 0; j < minLength; j++) {
            char c1 = word1.charAt(j);
            char c2 = word2.charAt(j);
            
            if (c1 != c2) {
                // Add edge c1 -> c2
                if (!graph.get(c1).contains(c2)) {
                    graph.get(c1).add(c2);
                    inDegree.put(c2, inDegree.get(c2) + 1);
                }
                break;
            }
        }
    }
    
    // Step 2: Topological sort using BFS
    StringBuilder result = new StringBuilder();
    Queue<Character> queue = new LinkedList<>();
    
    // Add all nodes with in-degree 0 to the queue
    for (char c : inDegree.keySet()) {
        if (inDegree.get(c) == 0) {
            queue.offer(c);
        }
    }
    
    while (!queue.isEmpty()) {
        char c = queue.poll();
        result.append(c);
        
        for (char neighbor : graph.get(c)) {
            inDegree.put(neighbor, inDegree.get(neighbor) - 1);
            if (inDegree.get(neighbor) == 0) {
                queue.offer(neighbor);
            }
        }
    }
    
    // Check if all nodes are visited
    if (result.length() != inDegree.size()) {
        return ""; // Cycle detected
    }
    
    return result.toString();
}
``` 