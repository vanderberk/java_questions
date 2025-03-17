# Course Schedule

**Difficulty**: Medium  
**Tags**: Depth-First Search, Breadth-First Search, Graph, Topological Sort

## Question
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]` indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

## Example Input/Output
**Input**: numCourses = 2, prerequisites = [[1,0]]  
**Output**: true  
**Explanation**: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

**Input**: numCourses = 2, prerequisites = [[1,0],[0,1]]  
**Output**: false  
**Explanation**: There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

## Solution Algorithm
This problem can be solved using graph algorithms. The courses and prerequisites form a directed graph where each course is a node, and a prerequisite relationship forms a directed edge.

We need to determine if the graph has a cycle. If there's a cycle, it means there's a circular dependency, and it's impossible to finish all courses. If there's no cycle, we can finish all courses.

We can use two approaches:

### Approach 1: Depth-First Search (DFS)
1. Build an adjacency list representation of the graph.
2. Use DFS to detect cycles in the graph.
3. For each node, mark it as "being visited" when we start exploring it.
4. If during DFS, we encounter a node that's "being visited," we've found a cycle.
5. After exploring all neighbors, mark the node as "visited."
6. If no cycles are found after exploring all nodes, return true.

### Approach 2: Topological Sort using BFS (Kahn's Algorithm)
1. Build an adjacency list and calculate the in-degree of each node.
2. Start with nodes that have an in-degree of 0 (no prerequisites).
3. For each node, decrement the in-degree of its neighbors.
4. If a neighbor's in-degree becomes 0, add it to the queue.
5. If we can visit all nodes, return true; otherwise, return false.

## Solution Code
```java
// Approach 1: DFS
public boolean canFinish(int numCourses, int[][] prerequisites) {
    // Build adjacency list
    List<List<Integer>> adjacencyList = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        adjacencyList.add(new ArrayList<>());
    }
    
    for (int[] prerequisite : prerequisites) {
        adjacencyList.get(prerequisite[0]).add(prerequisite[1]);
    }
    
    // 0 = unvisited, 1 = being visited, 2 = visited
    int[] visited = new int[numCourses];
    
    for (int i = 0; i < numCourses; i++) {
        if (visited[i] == 0 && hasCycle(i, adjacencyList, visited)) {
            return false;
        }
    }
    
    return true;
}

private boolean hasCycle(int course, List<List<Integer>> adjacencyList, int[] visited) {
    // Being visited
    visited[course] = 1;
    
    for (int prerequisite : adjacencyList.get(course)) {
        if (visited[prerequisite] == 1) {
            // Cycle detected
            return true;
        }
        
        if (visited[prerequisite] == 0 && hasCycle(prerequisite, adjacencyList, visited)) {
            return true;
        }
    }
    
    // Visited
    visited[course] = 2;
    return false;
}
```

```java
// Approach 2: BFS (Kahn's Algorithm)
public boolean canFinish(int numCourses, int[][] prerequisites) {
    // Build adjacency list
    List<List<Integer>> adjacencyList = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        adjacencyList.add(new ArrayList<>());
    }
    
    // Calculate in-degree of each node
    int[] inDegree = new int[numCourses];
    for (int[] prerequisite : prerequisites) {
        adjacencyList.get(prerequisite[1]).add(prerequisite[0]);
        inDegree[prerequisite[0]]++;
    }
    
    // Start with nodes that have no prerequisites
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }
    
    int count = 0;
    while (!queue.isEmpty()) {
        int course = queue.poll();
        count++;
        
        for (int nextCourse : adjacencyList.get(course)) {
            inDegree[nextCourse]--;
            if (inDegree[nextCourse] == 0) {
                queue.offer(nextCourse);
            }
        }
    }
    
    return count == numCourses;
}
``` 