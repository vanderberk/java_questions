# Tic Tac Toe

**Difficulty**: Easy  
**Tags**: Array, Matrix, Game, Design

## Question
Design a Tic-tac-toe game that is played between two players on an n x n grid. The game should support the following operations:

1. `move(int row, int col, int player)`: Places a move for the specified player (player 1 or 2) at the cell (row, col). The move is guaranteed to be a valid move, and the cell is previously empty.
2. The method should return the current game state after the move:
   - 0: No winner yet and there are still empty cells.
   - 1: Player 1 wins.
   - 2: Player 2 wins.
   - 3: Draw (no winner and no empty cells).

You should implement the game for a standard 3x3 board, but your solution should be extendable to an n x n board.

## Example Input/Output
```
TicTacToe game = new TicTacToe(3);

// Player 1 makes a move at (0, 0)
game.move(0, 0, 1); // Returns 0 (no winner)

// Player 2 makes a move at (0, 2)
game.move(0, 2, 2); // Returns 0 (no winner)

// Player 1 makes a move at (1, 1)
game.move(1, 1, 1); // Returns 0 (no winner)

// Player 2 makes a move at (2, 0)
game.move(2, 0, 2); // Returns 0 (no winner)

// Player 1 makes a move at (2, 2)
game.move(2, 2, 1); // Returns 1 (player 1 wins)
```

## Solution Algorithm
To efficiently check for a win after each move, we can keep track of the sum of moves in each row, column, and diagonal:

1. Initialize arrays to track the sum of moves in each row, column, and both diagonals.
2. When a player makes a move:
   - Update the corresponding row, column, and diagonal (if applicable) sums.
   - For player 1, add 1 to the sum.
   - For player 2, add -1 to the sum.
3. After each move, check if the player has won:
   - If any row, column, or diagonal sum equals n or -n, a player has won.
   - If all cells are filled and no player has won, it's a draw.

This approach has O(1) time complexity for each move operation, which is more efficient than checking the entire board after each move.

## Solution Code
```java
public class TicTacToe {
    private int n;
    private int[] rows;
    private int[] cols;
    private int diagonal;
    private int antiDiagonal;
    private int emptyCells;
    
    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        this.n = n;
        rows = new int[n];
        cols = new int[n];
        diagonal = 0;
        antiDiagonal = 0;
        emptyCells = n * n;
    }
    
    /**
     * Player {player} makes a move at ({row}, {col}).
     * @param row The row of the move.
     * @param col The column of the move.
     * @param player The player, can be either 1 or 2.
     * @return The current game state after the move:
     *         0: No winner yet and there are still empty cells.
     *         1: Player 1 wins.
     *         2: Player 2 wins.
     *         3: Draw (no winner and no empty cells).
     */
    public int move(int row, int col, int player) {
        // Decrement the count of empty cells
        emptyCells--;
        
        // Calculate the value to add based on the player
        int value = (player == 1) ? 1 : -1;
        
        // Update row and column sums
        rows[row] += value;
        cols[col] += value;
        
        // Update diagonal sum if the move is on the diagonal
        if (row == col) {
            diagonal += value;
        }
        
        // Update anti-diagonal sum if the move is on the anti-diagonal
        if (row + col == n - 1) {
            antiDiagonal += value;
        }
        
        // Check if the current player has won
        if (Math.abs(rows[row]) == n || Math.abs(cols[col]) == n || 
            Math.abs(diagonal) == n || Math.abs(antiDiagonal) == n) {
            return player;
        }
        
        // Check if it's a draw
        if (emptyCells == 0) {
            return 3;
        }
        
        // No winner yet
        return 0;
    }
    
    public static void main(String[] args) {
        TicTacToe game = new TicTacToe(3);
        
        // Example game
        System.out.println("Player 1 moves at (0, 0): " + game.move(0, 0, 1));
        System.out.println("Player 2 moves at (0, 2): " + game.move(0, 2, 2));
        System.out.println("Player 1 moves at (1, 1): " + game.move(1, 1, 1));
        System.out.println("Player 2 moves at (2, 0): " + game.move(2, 0, 2));
        System.out.println("Player 1 moves at (2, 2): " + game.move(2, 2, 1));
    }
}
```

```java
// Alternative implementation with a more complete game experience
public class TicTacToeGame {
    private int n;
    private char[][] board;
    private int emptyCells;
    
    public TicTacToeGame(int n) {
        this.n = n;
        board = new char[n][n];
        emptyCells = n * n;
        
        // Initialize the board with empty cells
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = ' ';
            }
        }
    }
    
    /**
     * Makes a move for the specified player.
     * @param row The row of the move (0-indexed).
     * @param col The column of the move (0-indexed).
     * @param player The player (1 for X, 2 for O).
     * @return The current game state.
     */
    public int move(int row, int col, int player) {
        if (row < 0 || row >= n || col < 0 || col >= n || board[row][col] != ' ') {
            throw new IllegalArgumentException("Invalid move");
        }
        
        // Place the player's mark on the board
        board[row][col] = (player == 1) ? 'X' : 'O';
        emptyCells--;
        
        // Check if the current player has won
        if (hasWon(row, col, player)) {
            return player;
        }
        
        // Check if it's a draw
        if (emptyCells == 0) {
            return 3;
        }
        
        // No winner yet
        return 0;
    }
    
    /**
     * Checks if the player has won after making a move.
     */
    private boolean hasWon(int row, int col, int player) {
        char mark = (player == 1) ? 'X' : 'O';
        
        // Check row
        boolean rowWin = true;
        for (int j = 0; j < n; j++) {
            if (board[row][j] != mark) {
                rowWin = false;
                break;
            }
        }
        if (rowWin) return true;
        
        // Check column
        boolean colWin = true;
        for (int i = 0; i < n; i++) {
            if (board[i][col] != mark) {
                colWin = false;
                break;
            }
        }
        if (colWin) return true;
        
        // Check diagonal
        if (row == col) {
            boolean diagWin = true;
            for (int i = 0; i < n; i++) {
                if (board[i][i] != mark) {
                    diagWin = false;
                    break;
                }
            }
            if (diagWin) return true;
        }
        
        // Check anti-diagonal
        if (row + col == n - 1) {
            boolean antiDiagWin = true;
            for (int i = 0; i < n; i++) {
                if (board[i][n - 1 - i] != mark) {
                    antiDiagWin = false;
                    break;
                }
            }
            if (antiDiagWin) return true;
        }
        
        return false;
    }
    
    /**
     * Prints the current state of the board.
     */
    public void printBoard() {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                System.out.print(board[i][j]);
                if (j < n - 1) {
                    System.out.print(" | ");
                }
            }
            System.out.println();
            if (i < n - 1) {
                for (int j = 0; j < n; j++) {
                    System.out.print("--");
                    if (j < n - 1) {
                        System.out.print("+");
                    }
                }
                System.out.println();
            }
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        TicTacToeGame game = new TicTacToeGame(3);
        
        // Example game
        System.out.println("Initial board:");
        game.printBoard();
        
        int result;
        
        result = game.move(0, 0, 1);
        System.out.println("Player 1 (X) moves at (0, 0). Result: " + getResultString(result));
        game.printBoard();
        
        result = game.move(0, 2, 2);
        System.out.println("Player 2 (O) moves at (0, 2). Result: " + getResultString(result));
        game.printBoard();
        
        result = game.move(1, 1, 1);
        System.out.println("Player 1 (X) moves at (1, 1). Result: " + getResultString(result));
        game.printBoard();
        
        result = game.move(2, 0, 2);
        System.out.println("Player 2 (O) moves at (2, 0). Result: " + getResultString(result));
        game.printBoard();
        
        result = game.move(2, 2, 1);
        System.out.println("Player 1 (X) moves at (2, 2). Result: " + getResultString(result));
        game.printBoard();
        
        System.out.println("Final result: " + getResultString(result));
    }
    
    private static String getResultString(int result) {
        switch (result) {
            case 0: return "No winner yet";
            case 1: return "Player 1 (X) wins";
            case 2: return "Player 2 (O) wins";
            case 3: return "Draw";
            default: return "Unknown";
        }
    }
}
```

```java
// Implementation with a complete interactive game
import java.util.Scanner;

public class InteractiveTicTacToe {
    private int n;
    private char[][] board;
    private int emptyCells;
    
    public InteractiveTicTacToe(int n) {
        this.n = n;
        board = new char[n][n];
        emptyCells = n * n;
        
        // Initialize the board with empty cells
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = ' ';
            }
        }
    }
    
    /**
     * Makes a move for the specified player.
     * @return The current game state.
     */
    public int move(int row, int col, int player) {
        if (row < 0 || row >= n || col < 0 || col >= n || board[row][col] != ' ') {
            return -1; // Invalid move
        }
        
        // Place the player's mark on the board
        board[row][col] = (player == 1) ? 'X' : 'O';
        emptyCells--;
        
        // Check if the current player has won
        if (hasWon(row, col, player)) {
            return player;
        }
        
        // Check if it's a draw
        if (emptyCells == 0) {
            return 3;
        }
        
        // No winner yet
        return 0;
    }
    
    /**
     * Checks if the player has won after making a move.
     */
    private boolean hasWon(int row, int col, int player) {
        char mark = (player == 1) ? 'X' : 'O';
        
        // Check row
        boolean rowWin = true;
        for (int j = 0; j < n; j++) {
            if (board[row][j] != mark) {
                rowWin = false;
                break;
            }
        }
        if (rowWin) return true;
        
        // Check column
        boolean colWin = true;
        for (int i = 0; i < n; i++) {
            if (board[i][col] != mark) {
                colWin = false;
                break;
            }
        }
        if (colWin) return true;
        
        // Check diagonal
        if (row == col) {
            boolean diagWin = true;
            for (int i = 0; i < n; i++) {
                if (board[i][i] != mark) {
                    diagWin = false;
                    break;
                }
            }
            if (diagWin) return true;
        }
        
        // Check anti-diagonal
        if (row + col == n - 1) {
            boolean antiDiagWin = true;
            for (int i = 0; i < n; i++) {
                if (board[i][n - 1 - i] != mark) {
                    antiDiagWin = false;
                    break;
                }
            }
            if (antiDiagWin) return true;
        }
        
        return false;
    }
    
    /**
     * Prints the current state of the board.
     */
    public void printBoard() {
        System.out.println();
        // Print column numbers
        System.out.print("  ");
        for (int j = 0; j < n; j++) {
            System.out.print(" " + j + " ");
            if (j < n - 1) {
                System.out.print("|");
            }
        }
        System.out.println();
        
        // Print separator
        System.out.print("  ");
        for (int j = 0; j < n; j++) {
            System.out.print("---");
            if (j < n - 1) {
                System.out.print("+");
            }
        }
        System.out.println();
        
        // Print rows
        for (int i = 0; i < n; i++) {
            System.out.print(i + " ");
            for (int j = 0; j < n; j++) {
                System.out.print(" " + board[i][j] + " ");
                if (j < n - 1) {
                    System.out.print("|");
                }
            }
            System.out.println();
            
            if (i < n - 1) {
                System.out.print("  ");
                for (int j = 0; j < n; j++) {
                    System.out.print("---");
                    if (j < n - 1) {
                        System.out.print("+");
                    }
                }
                System.out.println();
            }
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Welcome to Tic Tac Toe!");
        System.out.print("Enter the board size (e.g., 3 for a 3x3 board): ");
        int size = scanner.nextInt();
        
        InteractiveTicTacToe game = new InteractiveTicTacToe(size);
        int currentPlayer = 1;
        int gameState = 0;
        
        System.out.println("Game started! Player 1 is X, Player 2 is O.");
        game.printBoard();
        
        while (gameState == 0) {
            System.out.println("Player " + currentPlayer + "'s turn (" + (currentPlayer == 1 ? "X" : "O") + ")");
            
            int row, col;
            int moveResult;
            
            do {
                System.out.print("Enter row (0-" + (size - 1) + "): ");
                row = scanner.nextInt();
                
                System.out.print("Enter column (0-" + (size - 1) + "): ");
                col = scanner.nextInt();
                
                moveResult = game.move(row, col, currentPlayer);
                
                if (moveResult == -1) {
                    System.out.println("Invalid move! Try again.");
                }
            } while (moveResult == -1);
            
            game.printBoard();
            gameState = moveResult;
            
            // Switch players
            currentPlayer = (currentPlayer == 1) ? 2 : 1;
        }
        
        // Game over
        switch (gameState) {
            case 1:
                System.out.println("Player 1 (X) wins!");
                break;
            case 2:
                System.out.println("Player 2 (O) wins!");
                break;
            case 3:
                System.out.println("It's a draw!");
                break;
        }
        
        scanner.close();
    }
} 