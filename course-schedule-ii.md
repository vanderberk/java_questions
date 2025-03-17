# Course Schedule II

**Difficulty**: Medium  
**Tags**: Depth-First Search, Breadth-First Search, Graph, Topological Sort

## Question
There are a total of `n` courses you have to take, labeled from `0` to `n-1`. Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`.

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

## Example Input/Output
**Input**: n = 2, prerequisites = [[1,0]]  
**Output**: [0,1]  
**Explanation**: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

**Input**: n = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]  
**Output**: [0,1,2,3] or [0,2,1,3]  
**Explanation**: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

**Input**: n = 1, prerequisites = []  
**Output**: [0]

## Solution Algorithm
This problem is a classic application of topological sorting in a directed graph. We need to find an order of courses such that for each course, all its prerequisites are taken before it.

There are two common approaches to perform topological sorting:

### Approach 1: Kahn's Algorithm (BFS)
1. Build an adjacency list representation of the graph.
2. Calculate the in-degree (number of incoming edges) for each node.
3. Initialize a queue with all nodes that have an in-degree of 0 (courses with no prerequisites).
4. While the queue is not empty:
   - Dequeue a node and add it to the result.
   - Decrease the in-degree of all its neighbors by 1.
   - If a neighbor's in-degree becomes 0, enqueue it.
5. If the result contains all courses, return it. Otherwise, return an empty array (cycle detected).

### Approach 2: Depth-First Search (DFS) with Cycle Detection
1. Build an adjacency list representation of the graph.
2. Use DFS with three states for each node: unvisited, visiting, and visited.
3. If we encounter a node that is in the "visiting" state during DFS, there is a cycle.
4. Add nodes to the result in reverse order of their finishing time.
5. Reverse the result at the end.

## Solution Code
```java
// Approach 1: Kahn's Algorithm (BFS)
public int[] findOrder(int numCourses, int[][] prerequisites) {
    // Build the adjacency list
    List<List<Integer>> adjList = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        adjList.add(new ArrayList<>());
    }
    
    // Calculate in-degree for each node
    int[] inDegree = new int[numCourses];
    for (int[] prereq : prerequisites) {
        int course = prereq[0];
        int prerequisite = prereq[1];
        adjList.get(prerequisite).add(course);
        inDegree[course]++;
    }
    
    // Initialize queue with nodes that have no prerequisites
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }
    
    // Process the queue
    int[] result = new int[numCourses];
    int index = 0;
    
    while (!queue.isEmpty()) {
        int current = queue.poll();
        result[index++] = current;
        
        // Decrease in-degree of neighbors
        for (int neighbor : adjList.get(current)) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] == 0) {
                queue.offer(neighbor);
            }
        }
    }
    
    // Check if all courses can be taken
    if (index == numCourses) {
        return result;
    } else {
        return new int[0]; // Cycle detected
    }
}
```

```java
// Approach 2: DFS with Cycle Detection
public int[] findOrder(int numCourses, int[][] prerequisites) {
    // Build the adjacency list
    List<List<Integer>> adjList = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        adjList.add(new ArrayList<>());
    }
    
    for (int[] prereq : prerequisites) {
        int course = prereq[0];
        int prerequisite = prereq[1];
        adjList.get(prerequisite).add(course);
    }
    
    // 0: unvisited, 1: visiting, 2: visited
    int[] visited = new int[numCourses];
    List<Integer> result = new ArrayList<>();
    
    // DFS for each unvisited node
    for (int i = 0; i < numCourses; i++) {
        if (visited[i] == 0) {
            if (!dfs(adjList, visited, result, i)) {
                return new int[0]; // Cycle detected
            }
        }
    }
    
    // Convert list to array and reverse
    int[] order = new int[numCourses];
    for (int i = 0; i < numCourses; i++) {
        order[i] = result.get(numCourses - 1 - i);
    }
    
    return order;
}

private boolean dfs(List<List<Integer>> adjList, int[] visited, List<Integer> result, int node) {
    // Mark as visiting
    visited[node] = 1;
    
    // Visit all neighbors
    for (int neighbor : adjList.get(node)) {
        if (visited[neighbor] == 1) {
            return false; // Cycle detected
        }
        
        if (visited[neighbor] == 0) {
            if (!dfs(adjList, visited, result, neighbor)) {
                return false;
            }
        }
    }
    
    // Mark as visited
    visited[node] = 2;
    result.add(node);
    
    return true;
}
```

```java
// Alternative implementation with more descriptive variable names
public int[] findOrder(int numCourses, int[][] prerequisites) {
    // Create a graph representation
    Map<Integer, List<Integer>> graph = new HashMap<>();
    for (int i = 0; i < numCourses; i++) {
        graph.put(i, new ArrayList<>());
    }
    
    // Calculate in-degree for each course
    int[] inDegree = new int[numCourses];
    
    // Build the graph
    for (int[] prerequisite : prerequisites) {
        int course = prerequisite[0];
        int prereq = prerequisite[1];
        
        // Add an edge from prereq to course
        graph.get(prereq).add(course);
        
        // Increment in-degree of the course
        inDegree[course]++;
    }
    
    // Queue for BFS (start with courses that have no prerequisites)
    Queue<Integer> noPrereqCourses = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            noPrereqCourses.offer(i);
        }
    }
    
    // Array to store the course order
    int[] courseOrder = new int[numCourses];
    int coursesTaken = 0;
    
    // Process courses in topological order
    while (!noPrereqCourses.isEmpty()) {
        int currentCourse = noPrereqCourses.poll();
        courseOrder[coursesTaken++] = currentCourse;
        
        // For each course that depends on the current course
        for (int nextCourse : graph.get(currentCourse)) {
            // Decrement its in-degree
            inDegree[nextCourse]--;
            
            // If all prerequisites are satisfied, add to queue
            if (inDegree[nextCourse] == 0) {
                noPrereqCourses.offer(nextCourse);
            }
        }
    }
    
    // Check if all courses can be taken
    if (coursesTaken == numCourses) {
        return courseOrder;
    } else {
        // Cycle detected, impossible to finish all courses
        return new int[0];
    }
}
``` 