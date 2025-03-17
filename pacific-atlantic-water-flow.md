# Pacific Atlantic Water Flow

**Difficulty**: Medium  
**Tags**: Array, Depth-First Search, Breadth-First Search, Matrix

## Question
There is an `m x n` rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates `result` where `result[i] = [ri, ci]` denotes that rain water can flow from cell `(ri, ci)` to both the Pacific and Atlantic oceans.

## Example Input/Output
**Input**: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]  
**Output**: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]  
**Explanation**: The following cells can flow to both the Pacific and Atlantic oceans:
- [0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
- [1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
- [1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
- [2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
- [3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
- [3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
- [4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.

**Input**: heights = [[1]]  
**Output**: [[0,0]]  
**Explanation**: The water can flow from the only cell to the Pacific and Atlantic oceans.

## Solution Algorithm
We can solve this problem using either Depth-First Search (DFS) or Breadth-First Search (BFS). The key insight is to work backwards: instead of trying to find paths from each cell to both oceans, we start from the ocean borders and find all cells that can reach each ocean.

### Approach:
1. Create two boolean matrices to mark cells that can reach the Pacific and Atlantic oceans.
2. Start DFS/BFS from the cells bordering each ocean:
   - For the Pacific Ocean: cells in the first row and first column.
   - For the Atlantic Ocean: cells in the last row and last column.
3. During the search, mark cells that can be reached from the ocean borders.
4. The cells that can reach both oceans are the intersection of the two sets.

## Solution Code
```java
// DFS Approach
public List<List<Integer>> pacificAtlantic(int[][] heights) {
    List<List<Integer>> result = new ArrayList<>();
    if (heights == null || heights.length == 0 || heights[0].length == 0) {
        return result;
    }
    
    int rows = heights.length;
    int cols = heights[0].length;
    
    // Create matrices to track cells that can reach each ocean
    boolean[][] canReachPacific = new boolean[rows][cols];
    boolean[][] canReachAtlantic = new boolean[rows][cols];
    
    // DFS from Pacific borders (top and left)
    for (int i = 0; i < rows; i++) {
        dfs(heights, i, 0, canReachPacific, Integer.MIN_VALUE);
    }
    for (int j = 0; j < cols; j++) {
        dfs(heights, 0, j, canReachPacific, Integer.MIN_VALUE);
    }
    
    // DFS from Atlantic borders (bottom and right)
    for (int i = 0; i < rows; i++) {
        dfs(heights, i, cols - 1, canReachAtlantic, Integer.MIN_VALUE);
    }
    for (int j = 0; j < cols; j++) {
        dfs(heights, rows - 1, j, canReachAtlantic, Integer.MIN_VALUE);
    }
    
    // Find cells that can reach both oceans
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (canReachPacific[i][j] && canReachAtlantic[i][j]) {
                result.add(Arrays.asList(i, j));
            }
        }
    }
    
    return result;
}

private void dfs(int[][] heights, int row, int col, boolean[][] canReach, int prevHeight) {
    // Check if out of bounds or already visited or can't flow from previous cell
    if (row < 0 || row >= heights.length || col < 0 || col >= heights[0].length || 
        canReach[row][col] || heights[row][col] < prevHeight) {
        return;
    }
    
    // Mark as reachable
    canReach[row][col] = true;
    
    // Explore all four directions
    dfs(heights, row + 1, col, canReach, heights[row][col]);
    dfs(heights, row - 1, col, canReach, heights[row][col]);
    dfs(heights, row, col + 1, canReach, heights[row][col]);
    dfs(heights, row, col - 1, canReach, heights[row][col]);
}
```

```java
// BFS Approach
public List<List<Integer>> pacificAtlantic(int[][] heights) {
    List<List<Integer>> result = new ArrayList<>();
    if (heights == null || heights.length == 0 || heights[0].length == 0) {
        return result;
    }
    
    int rows = heights.length;
    int cols = heights[0].length;
    
    // Create matrices to track cells that can reach each ocean
    boolean[][] canReachPacific = new boolean[rows][cols];
    boolean[][] canReachAtlantic = new boolean[rows][cols];
    
    // Queues for BFS
    Queue<int[]> pacificQueue = new LinkedList<>();
    Queue<int[]> atlanticQueue = new LinkedList<>();
    
    // Add Pacific border cells to the queue
    for (int i = 0; i < rows; i++) {
        pacificQueue.offer(new int[]{i, 0});
        canReachPacific[i][0] = true;
        
        atlanticQueue.offer(new int[]{i, cols - 1});
        canReachAtlantic[i][cols - 1] = true;
    }
    
    for (int j = 0; j < cols; j++) {
        pacificQueue.offer(new int[]{0, j});
        canReachPacific[0][j] = true;
        
        atlanticQueue.offer(new int[]{rows - 1, j});
        canReachAtlantic[rows - 1][j] = true;
    }
    
    // Directions for BFS: up, right, down, left
    int[][] directions = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    
    // BFS from Pacific borders
    bfs(heights, pacificQueue, canReachPacific, directions);
    
    // BFS from Atlantic borders
    bfs(heights, atlanticQueue, canReachAtlantic, directions);
    
    // Find cells that can reach both oceans
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (canReachPacific[i][j] && canReachAtlantic[i][j]) {
                result.add(Arrays.asList(i, j));
            }
        }
    }
    
    return result;
}

private void bfs(int[][] heights, Queue<int[]> queue, boolean[][] canReach, int[][] directions) {
    int rows = heights.length;
    int cols = heights[0].length;
    
    while (!queue.isEmpty()) {
        int[] cell = queue.poll();
        int row = cell[0];
        int col = cell[1];
        
        // Explore all four directions
        for (int[] dir : directions) {
            int newRow = row + dir[0];
            int newCol = col + dir[1];
            
            // Check if out of bounds or already visited or can't flow from current cell
            if (newRow < 0 || newRow >= rows || newCol < 0 || newCol >= cols || 
                canReach[newRow][newCol] || heights[newRow][newCol] < heights[row][col]) {
                continue;
            }
            
            // Mark as reachable and add to queue
            canReach[newRow][newCol] = true;
            queue.offer(new int[]{newRow, newCol});
        }
    }
}
```

```java
// Alternative implementation with cleaner code
public List<List<Integer>> pacificAtlantic(int[][] heights) {
    List<List<Integer>> result = new ArrayList<>();
    if (heights == null || heights.length == 0 || heights[0].length == 0) {
        return result;
    }
    
    int rows = heights.length;
    int cols = heights[0].length;
    
    // Create matrices to track cells that can reach each ocean
    boolean[][] pacificReachable = new boolean[rows][cols];
    boolean[][] atlanticReachable = new boolean[rows][cols];
    
    // Process each border cell
    for (int i = 0; i < rows; i++) {
        // Pacific border (left)
        dfs(heights, i, 0, pacificReachable, heights[i][0]);
        // Atlantic border (right)
        dfs(heights, i, cols - 1, atlanticReachable, heights[i][cols - 1]);
    }
    
    for (int j = 0; j < cols; j++) {
        // Pacific border (top)
        dfs(heights, 0, j, pacificReachable, heights[0][j]);
        // Atlantic border (bottom)
        dfs(heights, rows - 1, j, atlanticReachable, heights[rows - 1][j]);
    }
    
    // Find cells that can reach both oceans
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (pacificReachable[i][j] && atlanticReachable[i][j]) {
                result.add(Arrays.asList(i, j));
            }
        }
    }
    
    return result;
}

private void dfs(int[][] heights, int row, int col, boolean[][] reachable, int prevHeight) {
    // Check if out of bounds or already visited or can't flow from previous cell
    if (row < 0 || row >= heights.length || col < 0 || col >= heights[0].length || 
        reachable[row][col] || heights[row][col] < prevHeight) {
        return;
    }
    
    // Mark as reachable
    reachable[row][col] = true;
    
    // Directions: up, right, down, left
    int[][] directions = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    
    // Explore all four directions
    for (int[] dir : directions) {
        dfs(heights, row + dir[0], col + dir[1], reachable, heights[row][col]);
    }
}
``` 