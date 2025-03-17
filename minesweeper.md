# Minesweeper

**Difficulty**: Medium  
**Tags**: Array, Matrix, DFS, Game

## Question
Design and implement a Minesweeper game. Minesweeper is a single-player puzzle game where the objective is to clear a rectangular board containing hidden "mines" without detonating any of them, with help from clues about the number of neighboring mines in each field.

Your implementation should include the following:

1. A method to initialize the game board with a specified width, height, and number of mines.
2. A method to reveal a cell at a given position. If the cell contains a mine, the game is over. If the cell is empty (has no adjacent mines), all adjacent cells should be revealed recursively.
3. A method to flag or unflag a cell as potentially containing a mine.
4. A method to check if the game is won (all non-mine cells are revealed).

## Example Input/Output
```
Minesweeper game = new Minesweeper(4, 4, 2);
// Initialize a 4x4 board with 2 mines

game.reveal(0, 0); // Reveals the cell at (0, 0)
// If it's not a mine, it shows the number of adjacent mines or reveals adjacent cells

game.flag(1, 1); // Flags the cell at (1, 1) as potentially containing a mine

game.isGameWon(); // Returns true if all non-mine cells are revealed, false otherwise
```

## Solution Algorithm
To implement the Minesweeper game, we need to:

1. **Initialize the board**:
   - Create a board of the specified dimensions.
   - Randomly place the specified number of mines on the board.
   - Calculate the number of adjacent mines for each cell.

2. **Reveal a cell**:
   - If the cell contains a mine, the game is over.
   - If the cell has adjacent mines, reveal only that cell.
   - If the cell has no adjacent mines, recursively reveal all adjacent cells.

3. **Flag a cell**:
   - Mark or unmark a cell as potentially containing a mine.

4. **Check if the game is won**:
   - The game is won if all non-mine cells are revealed.

We'll use a depth-first search (DFS) algorithm to recursively reveal adjacent cells when a cell with no adjacent mines is revealed.

## Solution Code
```java
import java.util.*;

public class Minesweeper {
    private int width;
    private int height;
    private int mineCount;
    private boolean[][] mines;
    private int[][] adjacentMines;
    private boolean[][] revealed;
    private boolean[][] flagged;
    private boolean gameOver;
    private int revealedCount;
    
    /**
     * Initialize the Minesweeper game.
     * @param width The width of the board.
     * @param height The height of the board.
     * @param mineCount The number of mines to place on the board.
     */
    public Minesweeper(int width, int height, int mineCount) {
        if (mineCount >= width * height) {
            throw new IllegalArgumentException("Too many mines for the board size");
        }
        
        this.width = width;
        this.height = height;
        this.mineCount = mineCount;
        this.mines = new boolean[height][width];
        this.adjacentMines = new int[height][width];
        this.revealed = new boolean[height][width];
        this.flagged = new boolean[height][width];
        this.gameOver = false;
        this.revealedCount = 0;
        
        // Place mines randomly
        placeMines();
        
        // Calculate adjacent mines
        calculateAdjacentMines();
    }
    
    /**
     * Place mines randomly on the board.
     */
    private void placeMines() {
        Random random = new Random();
        int minesPlaced = 0;
        
        while (minesPlaced < mineCount) {
            int row = random.nextInt(height);
            int col = random.nextInt(width);
            
            if (!mines[row][col]) {
                mines[row][col] = true;
                minesPlaced++;
            }
        }
    }
    
    /**
     * Calculate the number of adjacent mines for each cell.
     */
    private void calculateAdjacentMines() {
        int[] dr = {-1, -1, -1, 0, 0, 1, 1, 1};
        int[] dc = {-1, 0, 1, -1, 1, -1, 0, 1};
        
        for (int row = 0; row < height; row++) {
            for (int col = 0; col < width; col++) {
                if (mines[row][col]) {
                    continue; // Skip mine cells
                }
                
                int count = 0;
                for (int i = 0; i < 8; i++) {
                    int newRow = row + dr[i];
                    int newCol = col + dc[i];
                    
                    if (isValidCell(newRow, newCol) && mines[newRow][newCol]) {
                        count++;
                    }
                }
                
                adjacentMines[row][col] = count;
            }
        }
    }
    
    /**
     * Check if a cell is valid (within the board boundaries).
     */
    private boolean isValidCell(int row, int col) {
        return row >= 0 && row < height && col >= 0 && col < width;
    }
    
    /**
     * Reveal a cell at the specified position.
     * @param row The row of the cell.
     * @param col The column of the cell.
     * @return true if the game continues, false if the game is over.
     */
    public boolean reveal(int row, int col) {
        if (gameOver || !isValidCell(row, col) || revealed[row][col] || flagged[row][col]) {
            return !gameOver;
        }
        
        revealed[row][col] = true;
        revealedCount++;
        
        if (mines[row][col]) {
            gameOver = true;
            return false; // Game over
        }
        
        // If the cell has no adjacent mines, reveal all adjacent cells
        if (adjacentMines[row][col] == 0) {
            revealAdjacentCells(row, col);
        }
        
        return true;
    }
    
    /**
     * Recursively reveal adjacent cells for a cell with no adjacent mines.
     */
    private void revealAdjacentCells(int row, int col) {
        int[] dr = {-1, -1, -1, 0, 0, 1, 1, 1};
        int[] dc = {-1, 0, 1, -1, 1, -1, 0, 1};
        
        for (int i = 0; i < 8; i++) {
            int newRow = row + dr[i];
            int newCol = col + dc[i];
            
            if (isValidCell(newRow, newCol) && !revealed[newRow][newCol] && !flagged[newRow][newCol]) {
                revealed[newRow][newCol] = true;
                revealedCount++;
                
                if (adjacentMines[newRow][newCol] == 0) {
                    revealAdjacentCells(newRow, newCol);
                }
            }
        }
    }
    
    /**
     * Flag or unflag a cell as potentially containing a mine.
     * @param row The row of the cell.
     * @param col The column of the cell.
     * @return true if the cell was flagged or unflagged, false otherwise.
     */
    public boolean flag(int row, int col) {
        if (gameOver || !isValidCell(row, col) || revealed[row][col]) {
            return false;
        }
        
        flagged[row][col] = !flagged[row][col];
        return true;
    }
    
    /**
     * Check if the game is won.
     * @return true if all non-mine cells are revealed, false otherwise.
     */
    public boolean isGameWon() {
        return !gameOver && revealedCount == (width * height - mineCount);
    }
    
    /**
     * Get the current state of the board as a 2D character array.
     * - 'M': Mine (only shown when game is over)
     * - 'F': Flagged cell
     * - '0'-'8': Revealed cell with the number of adjacent mines
     * - '.': Unrevealed cell
     */
    public char[][] getBoardState() {
        char[][] board = new char[height][width];
        
        for (int row = 0; row < height; row++) {
            for (int col = 0; col < width; col++) {
                if (revealed[row][col]) {
                    if (mines[row][col]) {
                        board[row][col] = 'M'; // Mine
                    } else {
                        board[row][col] = (char) ('0' + adjacentMines[row][col]); // Number of adjacent mines
                    }
                } else if (flagged[row][col]) {
                    board[row][col] = 'F'; // Flagged
                } else {
                    board[row][col] = '.'; // Unrevealed
                }
            }
        }
        
        return board;
    }
    
    /**
     * Print the current state of the board.
     */
    public void printBoard() {
        char[][] board = getBoardState();
        
        // Print column numbers
        System.out.print("  ");
        for (int col = 0; col < width; col++) {
            System.out.print(col + " ");
        }
        System.out.println();
        
        // Print separator
        System.out.print("  ");
        for (int col = 0; col < width; col++) {
            System.out.print("--");
        }
        System.out.println();
        
        // Print board
        for (int row = 0; row < height; row++) {
            System.out.print(row + "| ");
            for (int col = 0; col < width; col++) {
                System.out.print(board[row][col] + " ");
            }
            System.out.println();
        }
    }
    
    /**
     * Reveal all mines on the board (used when the game is over).
     */
    public void revealAllMines() {
        for (int row = 0; row < height; row++) {
            for (int col = 0; col < width; col++) {
                if (mines[row][col]) {
                    revealed[row][col] = true;
                }
            }
        }
    }
    
    public static void main(String[] args) {
        // Create a 4x4 board with 2 mines
        Minesweeper game = new Minesweeper(4, 4, 2);
        
        System.out.println("Initial board:");
        game.printBoard();
        
        // Reveal a cell
        System.out.println("\nRevealing cell (0, 0):");
        boolean gameStatus = game.reveal(0, 0);
        game.printBoard();
        
        if (!gameStatus) {
            System.out.println("Game over! You hit a mine.");
            game.revealAllMines();
            game.printBoard();
        } else {
            // Flag a cell
            System.out.println("\nFlagging cell (1, 1):");
            game.flag(1, 1);
            game.printBoard();
            
            // Check if the game is won
            if (game.isGameWon()) {
                System.out.println("Congratulations! You won the game.");
            } else {
                System.out.println("Game in progress...");
            }
        }
    }
} 