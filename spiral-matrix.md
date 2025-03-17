# Spiral Matrix

**Difficulty**: Medium  
**Tags**: Array, Matrix, Simulation

## Question
Given an `m x n` matrix, return all elements of the matrix in spiral order.

## Example Input/Output
**Input**: 
```
matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]
```
**Output**: [1,2,3,6,9,8,7,4,5]

**Input**: 
```
matrix = [
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9, 10, 11, 12]
]
```
**Output**: [1,2,3,4,8,12,11,10,9,5,6,7]

## Solution Algorithm
We can solve this problem by simulating the spiral traversal:

1. Define four boundaries: `top`, `right`, `bottom`, and `left`.
2. Traverse the matrix in a spiral order by moving:
   - From left to right along the top row
   - From top to bottom along the rightmost column
   - From right to left along the bottom row
   - From bottom to top along the leftmost column
3. After each traversal, update the corresponding boundary.
4. Continue until all elements are visited.

Alternatively, we can use a direction-based approach:
1. Define a direction variable that cycles through right, down, left, and up.
2. Move in the current direction until we hit a boundary or a visited cell.
3. Change direction and continue.
4. Stop when all elements are visited.

## Solution Code
```java
// Approach 1: Boundary Traversal
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    
    int rows = matrix.length;
    int cols = matrix[0].length;
    
    int top = 0;
    int right = cols - 1;
    int bottom = rows - 1;
    int left = 0;
    
    while (top <= bottom && left <= right) {
        // Traverse right
        for (int j = left; j <= right; j++) {
            result.add(matrix[top][j]);
        }
        top++;
        
        // Traverse down
        for (int i = top; i <= bottom; i++) {
            result.add(matrix[i][right]);
        }
        right--;
        
        // Traverse left (if there are still rows to traverse)
        if (top <= bottom) {
            for (int j = right; j >= left; j--) {
                result.add(matrix[bottom][j]);
            }
            bottom--;
        }
        
        // Traverse up (if there are still columns to traverse)
        if (left <= right) {
            for (int i = bottom; i >= top; i--) {
                result.add(matrix[i][left]);
            }
            left++;
        }
    }
    
    return result;
}
```

```java
// Approach 2: Direction-based Traversal
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    
    int rows = matrix.length;
    int cols = matrix[0].length;
    int total = rows * cols;
    
    // Define directions: right, down, left, up
    int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int dirIndex = 0;
    
    // Starting position
    int row = 0;
    int col = 0;
    
    // Mark visited cells
    boolean[][] visited = new boolean[rows][cols];
    
    for (int i = 0; i < total; i++) {
        result.add(matrix[row][col]);
        visited[row][col] = true;
        
        // Calculate next position
        int nextRow = row + directions[dirIndex][0];
        int nextCol = col + directions[dirIndex][1];
        
        // Change direction if next position is out of bounds or already visited
        if (nextRow < 0 || nextRow >= rows || nextCol < 0 || nextCol >= cols || visited[nextRow][nextCol]) {
            dirIndex = (dirIndex + 1) % 4;
            nextRow = row + directions[dirIndex][0];
            nextCol = col + directions[dirIndex][1];
        }
        
        row = nextRow;
        col = nextCol;
    }
    
    return result;
}
``` 