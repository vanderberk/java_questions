# Number of Islands

**Difficulty**: Medium  
**Tags**: Array, Depth-First Search, Breadth-First Search, Union Find, Matrix

## Question
Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

## Example Input/Output
**Input**: 
```
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
```
**Output**: 1

**Input**: 
```
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```
**Output**: 3

## Solution Algorithm
We can solve this problem using several approaches:

### Approach 1: Depth-First Search (DFS)
1. Iterate through each cell in the grid.
2. When we find a cell with value '1', we increment our island count and perform a DFS to mark all connected land cells as visited (by changing their values to '0' or using a separate visited array).
3. The DFS explores all adjacent land cells (horizontally and vertically) and marks them as visited.
4. After the DFS completes, we've marked all cells of the current island as visited, so we won't count them again.
5. Continue the iteration and repeat the process for any unvisited land cells.

### Approach 2: Breadth-First Search (BFS)
1. Similar to DFS, we iterate through each cell in the grid.
2. When we find a cell with value '1', we increment our island count and perform a BFS to mark all connected land cells as visited.
3. The BFS uses a queue to explore all adjacent land cells level by level.
4. After the BFS completes, we've marked all cells of the current island as visited.
5. Continue the iteration and repeat the process for any unvisited land cells.

### Approach 3: Union Find (Disjoint Set)
1. Initialize a Union Find data structure with each land cell as a separate set.
2. Iterate through the grid and for each land cell, union it with its adjacent land cells.
3. The number of islands is the number of distinct sets in the Union Find structure.

## Solution Code
```java
// Approach 1: DFS
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    
    int numRows = grid.length;
    int numCols = grid[0].length;
    int count = 0;
    
    for (int r = 0; r < numRows; r++) {
        for (int c = 0; c < numCols; c++) {
            if (grid[r][c] == '1') {
                count++;
                dfs(grid, r, c);
            }
        }
    }
    
    return count;
}

private void dfs(char[][] grid, int r, int c) {
    int numRows = grid.length;
    int numCols = grid[0].length;
    
    // Check boundaries and if current cell is land
    if (r < 0 || c < 0 || r >= numRows || c >= numCols || grid[r][c] == '0') {
        return;
    }
    
    // Mark current cell as visited
    grid[r][c] = '0';
    
    // Explore adjacent cells
    dfs(grid, r - 1, c); // Up
    dfs(grid, r + 1, c); // Down
    dfs(grid, r, c - 1); // Left
    dfs(grid, r, c + 1); // Right
}
```

```java
// Approach 2: BFS
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    
    int numRows = grid.length;
    int numCols = grid[0].length;
    int count = 0;
    
    for (int r = 0; r < numRows; r++) {
        for (int c = 0; c < numCols; c++) {
            if (grid[r][c] == '1') {
                count++;
                bfs(grid, r, c);
            }
        }
    }
    
    return count;
}

private void bfs(char[][] grid, int r, int c) {
    int numRows = grid.length;
    int numCols = grid[0].length;
    
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[]{r, c});
    grid[r][c] = '0'; // Mark as visited
    
    int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}}; // Up, Down, Left, Right
    
    while (!queue.isEmpty()) {
        int[] curr = queue.poll();
        
        for (int[] dir : directions) {
            int newR = curr[0] + dir[0];
            int newC = curr[1] + dir[1];
            
            if (newR >= 0 && newC >= 0 && newR < numRows && newC < numCols && grid[newR][newC] == '1') {
                queue.offer(new int[]{newR, newC});
                grid[newR][newC] = '0'; // Mark as visited
            }
        }
    }
}
```

```java
// Approach 3: Union Find
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    
    int numRows = grid.length;
    int numCols = grid[0].length;
    
    UnionFind uf = new UnionFind(grid);
    
    for (int r = 0; r < numRows; r++) {
        for (int c = 0; c < numCols; c++) {
            if (grid[r][c] == '1') {
                grid[r][c] = '0'; // Mark as visited
                
                // Check adjacent cells and union if they are land
                if (r - 1 >= 0 && grid[r - 1][c] == '1') {
                    uf.union(r * numCols + c, (r - 1) * numCols + c);
                }
                if (r + 1 < numRows && grid[r + 1][c] == '1') {
                    uf.union(r * numCols + c, (r + 1) * numCols + c);
                }
                if (c - 1 >= 0 && grid[r][c - 1] == '1') {
                    uf.union(r * numCols + c, r * numCols + c - 1);
                }
                if (c + 1 < numCols && grid[r][c + 1] == '1') {
                    uf.union(r * numCols + c, r * numCols + c + 1);
                }
            }
        }
    }
    
    return uf.getCount();
}

class UnionFind {
    private int count; // Number of connected components
    private int[] parent;
    private int[] rank;
    
    public UnionFind(char[][] grid) {
        int numRows = grid.length;
        int numCols = grid[0].length;
        
        parent = new int[numRows * numCols];
        rank = new int[numRows * numCols];
        
        for (int r = 0; r < numRows; r++) {
            for (int c = 0; c < numCols; c++) {
                if (grid[r][c] == '1') {
                    parent[r * numCols + c] = r * numCols + c;
                    count++;
                }
            }
        }
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
            count--;
        }
    }
    
    public int getCount() {
        return count;
    }
}
``` 