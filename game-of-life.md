# Game of Life

**Difficulty**: Medium  
**Tags**: Array, Matrix, Simulation, Game

## Question
Implement Conway's Game of Life, a cellular automaton devised by mathematician John Conway. The game is played on an infinite two-dimensional grid of cells, where each cell can be either alive or dead. The state of each cell evolves over discrete time steps according to a set of rules based on the states of its eight neighbors.

Given an m x n grid representing the current state (0 for dead, 1 for alive), compute the next state of the grid after one iteration.

The rules of the game are:
1. Any live cell with fewer than two live neighbors dies (underpopulation).
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies (overpopulation).
4. Any dead cell with exactly three live neighbors becomes a live cell (reproduction).

Implement the solution in-place, i.e., without using an additional grid.

## Example Input/Output
**Input**:
```
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
```

**Output**:
```
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

## Solution Algorithm
To solve this problem in-place, we need to update the grid without using an additional data structure. However, we need to keep track of both the original state and the new state of each cell. We can use a clever encoding to achieve this:

1. Use a 2-bit representation for each cell:
   - The first bit (LSB) represents the current state.
   - The second bit represents the next state.

2. For each cell in the grid:
   - Count the number of live neighbors based on the first bit (original state).
   - Determine the next state based on the rules.
   - Store the next state in the second bit.

3. After processing all cells, shift the values right by 1 bit to get the final state.

This approach allows us to update the grid in-place without losing the original information.

## Solution Code
```java
public class GameOfLife {
    public void gameOfLife(int[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return;
        }
        
        int m = board.length;
        int n = board[0].length;
        
        // Directions for the 8 neighbors
        int[] dx = {-1, -1, -1, 0, 0, 1, 1, 1};
        int[] dy = {-1, 0, 1, -1, 1, -1, 0, 1};
        
        // First pass: mark the cells
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int liveNeighbors = 0;
                
                // Count live neighbors
                for (int k = 0; k < 8; k++) {
                    int newX = i + dx[k];
                    int newY = j + dy[k];
                    
                    if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                        // Check if the neighbor was originally alive (first bit is 1)
                        liveNeighbors += (board[newX][newY] & 1);
                    }
                }
                
                // Apply the rules and store the result in the second bit
                if (board[i][j] == 1) {
                    // Cell is currently alive
                    if (liveNeighbors == 2 || liveNeighbors == 3) {
                        // Cell stays alive
                        board[i][j] = 3; // 11 in binary (both current and next state are 1)
                    }
                    // Otherwise, cell dies (next state is 0, current state is 1, so value remains 1)
                } else {
                    // Cell is currently dead
                    if (liveNeighbors == 3) {
                        // Cell becomes alive
                        board[i][j] = 2; // 10 in binary (current state is 0, next state is 1)
                    }
                    // Otherwise, cell stays dead (both current and next state are 0, so value remains 0)
                }
            }
        }
        
        // Second pass: update the board
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // Shift right by 1 bit to get the next state
                board[i][j] >>= 1;
            }
        }
    }
    
    public static void main(String[] args) {
        GameOfLife game = new GameOfLife();
        int[][] board = {
            {0, 1, 0},
            {0, 0, 1},
            {1, 1, 1},
            {0, 0, 0}
        };
        
        System.out.println("Initial State:");
        printBoard(board);
        
        game.gameOfLife(board);
        
        System.out.println("Next State:");
        printBoard(board);
    }
    
    private static void printBoard(int[][] board) {
        for (int[] row : board) {
            for (int cell : row) {
                System.out.print(cell + " ");
            }
            System.out.println();
        }
        System.out.println();
    }
}
```

```java
// Alternative implementation with clearer variable names and comments
public class GameOfLife {
    // Cell states
    private static final int DEAD = 0;
    private static final int ALIVE = 1;
    private static final int DEAD_TO_ALIVE = 2;  // Dead in current state, alive in next state
    private static final int ALIVE_TO_ALIVE = 3; // Alive in current state, alive in next state
    
    public void gameOfLife(int[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return;
        }
        
        int rows = board.length;
        int cols = board[0].length;
        
        // Process each cell
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                int liveNeighbors = countLiveNeighbors(board, row, col, rows, cols);
                
                // Apply the rules of the game
                if (board[row][col] == ALIVE) {
                    // Rule 1 & 3: Any live cell with < 2 or > 3 live neighbors dies
                    // (We don't need to change the state as it will be 1 -> 0, which is ALIVE -> DEAD = 1)
                    
                    // Rule 2: Any live cell with 2 or 3 live neighbors lives on
                    if (liveNeighbors == 2 || liveNeighbors == 3) {
                        board[row][col] = ALIVE_TO_ALIVE; // 3 (11 in binary)
                    }
                } else {
                    // Rule 4: Any dead cell with exactly 3 live neighbors becomes alive
                    if (liveNeighbors == 3) {
                        board[row][col] = DEAD_TO_ALIVE; // 2 (10 in binary)
                    }
                }
            }
        }
        
        // Update the board to the next state
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                // Right shift by 1 to get the next state
                board[row][col] >>= 1;
            }
        }
    }
    
    private int countLiveNeighbors(int[][] board, int row, int col, int rows, int cols) {
        int liveNeighbors = 0;
        
        // Check all 8 neighboring cells
        for (int i = Math.max(0, row - 1); i <= Math.min(rows - 1, row + 1); i++) {
            for (int j = Math.max(0, col - 1); j <= Math.min(cols - 1, col + 1); j++) {
                // Skip the current cell
                if (i == row && j == col) {
                    continue;
                }
                
                // Check if the neighbor was originally alive (first bit is 1)
                liveNeighbors += (board[i][j] & 1);
            }
        }
        
        return liveNeighbors;
    }
    
    // Method to run the game for multiple generations
    public void runGenerations(int[][] board, int generations) {
        System.out.println("Initial State (Generation 0):");
        printBoard(board);
        
        for (int gen = 1; gen <= generations; gen++) {
            gameOfLife(board);
            System.out.println("Generation " + gen + ":");
            printBoard(board);
        }
    }
    
    private void printBoard(int[][] board) {
        for (int[] row : board) {
            for (int cell : row) {
                System.out.print(cell == ALIVE ? "■ " : "□ ");
            }
            System.out.println();
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        GameOfLife game = new GameOfLife();
        
        // Glider pattern
        int[][] board = {
            {0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 1, 0, 0, 0, 0, 0},
            {0, 0, 0, 1, 0, 0, 0, 0},
            {0, 1, 1, 1, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 0, 0}
        };
        
        game.runGenerations(board, 5);
    }
} 