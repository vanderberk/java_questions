# Sudoku Solver

**Difficulty**: Hard  
**Tags**: Array, Backtracking, Matrix

## Question
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:
1. Each of the digits 1-9 must occur exactly once in each row.
2. Each of the digits 1-9 must occur exactly once in each column.
3. Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

The '.' character indicates empty cells.

## Example Input/Output
**Input**: 
```
board = [
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```

**Output**: 
```
[
  ["5","3","4","6","7","8","9","1","2"],
  ["6","7","2","1","9","5","3","4","8"],
  ["1","9","8","3","4","2","5","6","7"],
  ["8","5","9","7","6","1","4","2","3"],
  ["4","2","6","8","5","3","7","9","1"],
  ["7","1","3","9","2","4","8","5","6"],
  ["9","6","1","5","3","7","2","8","4"],
  ["2","8","7","4","1","9","6","3","5"],
  ["3","4","5","2","8","6","1","7","9"]
]
```

## Solution Algorithm
This problem can be solved using backtracking:

1. Find an empty cell (denoted by '.').
2. Try placing digits 1-9 in the empty cell.
3. For each digit, check if it's valid to place it in the current cell by ensuring it doesn't violate any of the Sudoku rules.
4. If the digit is valid, place it in the cell and recursively try to solve the rest of the puzzle.
5. If the recursive call returns true, the puzzle is solved.
6. If the recursive call returns false, backtrack by removing the digit and try the next digit.
7. If all digits have been tried and none work, return false to trigger backtracking.

### Optimizations:
- Use arrays or sets to keep track of digits already used in each row, column, and 3x3 sub-box.
- Start with cells that have fewer valid options to reduce the search space.

## Solution Code
```java
public void solveSudoku(char[][] board) {
    solve(board);
}

private boolean solve(char[][] board) {
    for (int row = 0; row < 9; row++) {
        for (int col = 0; col < 9; col++) {
            if (board[row][col] == '.') {
                for (char digit = '1'; digit <= '9'; digit++) {
                    if (isValid(board, row, col, digit)) {
                        board[row][col] = digit;
                        
                        if (solve(board)) {
                            return true;
                        }
                        
                        board[row][col] = '.'; // Backtrack
                    }
                }
                return false; // No valid digit found
            }
        }
    }
    return true; // All cells filled
}

private boolean isValid(char[][] board, int row, int col, char digit) {
    // Check row
    for (int j = 0; j < 9; j++) {
        if (board[row][j] == digit) {
            return false;
        }
    }
    
    // Check column
    for (int i = 0; i < 9; i++) {
        if (board[i][col] == digit) {
            return false;
        }
    }
    
    // Check 3x3 sub-box
    int boxRow = (row / 3) * 3;
    int boxCol = (col / 3) * 3;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[boxRow + i][boxCol + j] == digit) {
                return false;
            }
        }
    }
    
    return true;
}
```

```java
// Optimized solution using arrays to track used digits
public void solveSudoku(char[][] board) {
    boolean[][] rowUsed = new boolean[9][9];
    boolean[][] colUsed = new boolean[9][9];
    boolean[][] boxUsed = new boolean[9][9];
    
    // Initialize the tracking arrays
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] != '.') {
                int digit = board[i][j] - '1';
                int boxIndex = (i / 3) * 3 + j / 3;
                rowUsed[i][digit] = true;
                colUsed[j][digit] = true;
                boxUsed[boxIndex][digit] = true;
            }
        }
    }
    
    solve(board, rowUsed, colUsed, boxUsed, 0, 0);
}

private boolean solve(char[][] board, boolean[][] rowUsed, boolean[][] colUsed, boolean[][] boxUsed, int row, int col) {
    // Move to the next row if we've processed all columns
    if (col == 9) {
        col = 0;
        row++;
        if (row == 9) {
            return true; // Puzzle solved
        }
    }
    
    // Skip filled cells
    if (board[row][col] != '.') {
        return solve(board, rowUsed, colUsed, boxUsed, row, col + 1);
    }
    
    // Try each digit
    for (int digit = 0; digit < 9; digit++) {
        int boxIndex = (row / 3) * 3 + col / 3;
        if (!rowUsed[row][digit] && !colUsed[col][digit] && !boxUsed[boxIndex][digit]) {
            // Place the digit
            board[row][col] = (char) ('1' + digit);
            rowUsed[row][digit] = true;
            colUsed[col][digit] = true;
            boxUsed[boxIndex][digit] = true;
            
            if (solve(board, rowUsed, colUsed, boxUsed, row, col + 1)) {
                return true;
            }
            
            // Backtrack
            board[row][col] = '.';
            rowUsed[row][digit] = false;
            colUsed[col][digit] = false;
            boxUsed[boxIndex][digit] = false;
        }
    }
    
    return false;
}
```

```java
// Alternative solution with more explicit variable names and comments
public void solveSudoku(char[][] board) {
    // Arrays to track which digits are already used in each row, column, and box
    boolean[][] rowDigits = new boolean[9][9]; // rowDigits[r][d] means digit d+1 is used in row r
    boolean[][] colDigits = new boolean[9][9]; // colDigits[c][d] means digit d+1 is used in column c
    boolean[][] boxDigits = new boolean[9][9]; // boxDigits[b][d] means digit d+1 is used in box b
    
    // Initialize tracking arrays based on the initial board state
    for (int row = 0; row < 9; row++) {
        for (int col = 0; col < 9; col++) {
            if (board[row][col] != '.') {
                int digit = board[row][col] - '1'; // Convert char '1'-'9' to int 0-8
                int boxIndex = (row / 3) * 3 + col / 3;
                
                rowDigits[row][digit] = true;
                colDigits[col][digit] = true;
                boxDigits[boxIndex][digit] = true;
            }
        }
    }
    
    // Start the backtracking process
    backtrack(board, rowDigits, colDigits, boxDigits, 0, 0);
}

private boolean backtrack(char[][] board, boolean[][] rowDigits, boolean[][] colDigits, 
                         boolean[][] boxDigits, int row, int col) {
    // If we've filled all rows, the puzzle is solved
    if (row == 9) {
        return true;
    }
    
    // Move to the next row when we finish a column
    if (col == 9) {
        return backtrack(board, rowDigits, colDigits, boxDigits, row + 1, 0);
    }
    
    // Skip cells that are already filled
    if (board[row][col] != '.') {
        return backtrack(board, rowDigits, colDigits, boxDigits, row, col + 1);
    }
    
    // Try each possible digit
    for (int digit = 0; digit < 9; digit++) {
        int boxIndex = (row / 3) * 3 + col / 3;
        
        // Check if the digit can be placed in this cell
        if (!rowDigits[row][digit] && !colDigits[col][digit] && !boxDigits[boxIndex][digit]) {
            // Place the digit
            board[row][col] = (char) ('1' + digit);
            rowDigits[row][digit] = true;
            colDigits[col][digit] = true;
            boxDigits[boxIndex][digit] = true;
            
            // Recursively try to solve the rest of the puzzle
            if (backtrack(board, rowDigits, colDigits, boxDigits, row, col + 1)) {
                return true;
            }
            
            // Backtrack if the current placement doesn't lead to a solution
            board[row][col] = '.';
            rowDigits[row][digit] = false;
            colDigits[col][digit] = false;
            boxDigits[boxIndex][digit] = false;
        }
    }
    
    // No valid digit found for this cell
    return false;
}
``` 