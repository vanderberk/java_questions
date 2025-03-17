# Critical Connections in a Network

**Difficulty**: Hard  
**Tags**: Depth-First Search, Graph, Biconnected Components, Tarjan's Algorithm

## Question
There are `n` servers numbered from `0` to `n-1` connected by undirected server-to-server connections forming a network where connections[i] = [a, b] represents a connection between servers a and b. Any server can reach any other server directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some server unable to reach some other server.

Return all critical connections in the network in any order.

## Example Input/Output
**Input**: n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]  
**Output**: [[1,3]]  
**Explanation**: The connection between servers 1 and 3 is a critical connection. If it's removed, server 3 will be unable to reach servers 0 and 2.

**Input**: n = 2, connections = [[0,1]]  
**Output**: [[0,1]]  
**Explanation**: The connection between servers 0 and 1 is a critical connection. If it's removed, the servers will be disconnected.

## Solution Algorithm
This problem is asking for bridges in an undirected graph. A bridge is an edge that, if removed, increases the number of connected components in the graph. We can use Tarjan's algorithm to find all bridges in the graph:

1. Perform a depth-first search (DFS) traversal of the graph.
2. For each node, keep track of:
   - `discovery[v]`: The time at which node v is first discovered.
   - `low[v]`: The lowest discovery time of any node that can be reached from the subtree rooted at v.
3. During the DFS, for each edge (u, v):
   - If v is not visited, recursively visit v.
   - After visiting v, update `low[u] = min(low[u], low[v])`.
   - If `low[v] > discovery[u]`, then the edge (u, v) is a bridge.
4. Return all bridges found.

The time complexity is O(V + E), where V is the number of vertices (servers) and E is the number of edges (connections).

## Solution Code
```java
import java.util.*;

class Solution {
    private List<List<Integer>> result;
    private List<Integer>[] graph;
    private int[] discovery;
    private int[] low;
    private int time;
    
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        result = new ArrayList<>();
        graph = new ArrayList[n];
        discovery = new int[n];
        low = new int[n];
        time = 0;
        
        // Initialize the graph
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
            discovery[i] = -1; // -1 indicates not visited
        }
        
        // Build the graph
        for (List<Integer> connection : connections) {
            int u = connection.get(0);
            int v = connection.get(1);
            graph[u].add(v);
            graph[v].add(u);
        }
        
        // Perform DFS from each unvisited node
        for (int i = 0; i < n; i++) {
            if (discovery[i] == -1) {
                dfs(i, -1); // Start DFS from node i with no parent
            }
        }
        
        return result;
    }
    
    private void dfs(int u, int parent) {
        // Mark the current node as visited
        discovery[u] = low[u] = ++time;
        
        // Visit all neighbors
        for (int v : graph[u]) {
            // Skip the parent
            if (v == parent) {
                continue;
            }
            
            // If v is not visited yet, make it a child of u in DFS tree
            if (discovery[v] == -1) {
                dfs(v, u);
                
                // Check if the subtree rooted at v has a connection to an ancestor of u
                low[u] = Math.min(low[u], low[v]);
                
                // If the lowest vertex reachable from subtree under v is below u in DFS tree, then u-v is a bridge
                if (low[v] > discovery[u]) {
                    result.add(Arrays.asList(u, v));
                }
            } else {
                // Update low value of u for parent function calls
                low[u] = Math.min(low[u], discovery[v]);
            }
        }
    }
}
```

```java
// Alternative implementation with more detailed comments
import java.util.*;

class Solution {
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        // Build the graph as an adjacency list
        List<Integer>[] graph = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }
        
        for (List<Integer> connection : connections) {
            int u = connection.get(0);
            int v = connection.get(1);
            graph[u].add(v);
            graph[v].add(u);
        }
        
        // Initialize arrays for Tarjan's algorithm
        int[] discoveryTime = new int[n]; // Time when a node is first discovered
        int[] lowestTime = new int[n];    // Lowest discovery time reachable from the node
        Arrays.fill(discoveryTime, -1);   // -1 indicates not visited
        
        List<List<Integer>> bridges = new ArrayList<>();
        
        // Start DFS from node 0
        dfs(0, -1, 0, discoveryTime, lowestTime, graph, bridges);
        
        return bridges;
    }
    
    private void dfs(int node, int parent, int time, int[] discoveryTime, int[] lowestTime, 
                    List<Integer>[] graph, List<List<Integer>> bridges) {
        // Mark the current node as visited with the current time
        discoveryTime[node] = lowestTime[node] = time;
        
        // Visit all neighbors
        for (int neighbor : graph[node]) {
            // Skip the parent
            if (neighbor == parent) {
                continue;
            }
            
            // If neighbor is not visited yet
            if (discoveryTime[neighbor] == -1) {
                // Recursively visit the neighbor
                dfs(neighbor, node, time + 1, discoveryTime, lowestTime, graph, bridges);
                
                // After visiting the neighbor, update the lowest time reachable from the current node
                lowestTime[node] = Math.min(lowestTime[node], lowestTime[neighbor]);
                
                // If the lowest time reachable from the neighbor is greater than the discovery time of the current node,
                // then the edge between them is a bridge
                if (lowestTime[neighbor] > discoveryTime[node]) {
                    bridges.add(Arrays.asList(node, neighbor));
                }
            } else {
                // If the neighbor is already visited, update the lowest time reachable from the current node
                lowestTime[node] = Math.min(lowestTime[node], discoveryTime[neighbor]);
            }
        }
    }
}
```

```java
// Implementation with a more explicit graph representation
import java.util.*;

class Solution {
    static class Graph {
        private int vertices;
        private List<Integer>[] adjacencyList;
        
        public Graph(int vertices) {
            this.vertices = vertices;
            this.adjacencyList = new ArrayList[vertices];
            for (int i = 0; i < vertices; i++) {
                adjacencyList[i] = new ArrayList<>();
            }
        }
        
        public void addEdge(int u, int v) {
            adjacencyList[u].add(v);
            adjacencyList[v].add(u);
        }
        
        public List<Integer> getNeighbors(int vertex) {
            return adjacencyList[vertex];
        }
    }
    
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        // Build the graph
        Graph graph = new Graph(n);
        for (List<Integer> connection : connections) {
            graph.addEdge(connection.get(0), connection.get(1));
        }
        
        // Initialize arrays for Tarjan's algorithm
        int[] discoveryTime = new int[n];
        int[] lowestTime = new int[n];
        boolean[] visited = new boolean[n];
        List<List<Integer>> bridges = new ArrayList<>();
        
        // Start DFS from node 0
        findBridges(graph, 0, -1, 0, discoveryTime, lowestTime, visited, bridges);
        
        return bridges;
    }
    
    private void findBridges(Graph graph, int node, int parent, int time, int[] discoveryTime, 
                            int[] lowestTime, boolean[] visited, List<List<Integer>> bridges) {
        // Mark the current node as visited
        visited[node] = true;
        discoveryTime[node] = lowestTime[node] = time;
        
        // Visit all neighbors
        for (int neighbor : graph.getNeighbors(node)) {
            // Skip the parent
            if (neighbor == parent) {
                continue;
            }
            
            // If neighbor is not visited yet
            if (!visited[neighbor]) {
                findBridges(graph, neighbor, node, time + 1, discoveryTime, lowestTime, visited, bridges);
                
                // Update the lowest time reachable from the current node
                lowestTime[node] = Math.min(lowestTime[node], lowestTime[neighbor]);
                
                // Check if the edge is a bridge
                if (lowestTime[neighbor] > discoveryTime[node]) {
                    bridges.add(Arrays.asList(node, neighbor));
                }
            } else {
                // Update the lowest time reachable from the current node
                lowestTime[node] = Math.min(lowestTime[node], discoveryTime[neighbor]);
            }
        }
    }
} 