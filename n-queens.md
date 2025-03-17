# N-Queens

**Difficulty**: Hard  
**Tags**: Array, Backtracking, Matrix

## Question
The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

## Example Input/Output
**Input**: n = 4  
**Output**: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]  
**Explanation**: There exist two distinct solutions to the 4-queens puzzle as shown above.

**Input**: n = 1  
**Output**: [["Q"]]

## Solution Algorithm
This problem can be solved using backtracking. The key insight is that each row and each column can only contain one queen. We can place queens row by row, and for each row, we try to place a queen in each column.

### Approach:
1. Start with an empty board.
2. Place a queen in the first row, then recursively try to place queens in the subsequent rows.
3. For each position, check if it's safe to place a queen by ensuring it doesn't attack any previously placed queens.
4. If a valid solution is found, add it to the result.
5. Backtrack by removing the queen and trying the next position.

### Optimizations:
- Use separate sets or arrays to keep track of occupied columns, diagonals, and anti-diagonals.
- For a position (row, col), the diagonal is identified by (row - col) and the anti-diagonal by (row + col).

## Solution Code
```java
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    
    // Initialize the board with empty spaces
    for (int i = 0; i < n; i++) {
        Arrays.fill(board[i], '.');
    }
    
    backtrack(board, 0, result);
    
    return result;
}

private void backtrack(char[][] board, int row, List<List<String>> result) {
    if (row == board.length) {
        // Found a valid solution
        result.add(constructSolution(board));
        return;
    }
    
    for (int col = 0; col < board.length; col++) {
        if (isValid(board, row, col)) {
            // Place the queen
            board[row][col] = 'Q';
            
            // Recursively place queens in the next rows
            backtrack(board, row + 1, result);
            
            // Backtrack
            board[row][col] = '.';
        }
    }
}

private boolean isValid(char[][] board, int row, int col) {
    // Check if there is a queen in the same column
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'Q') {
            return false;
        }
    }
    
    // Check if there is a queen in the upper-left diagonal
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') {
            return false;
        }
    }
    
    // Check if there is a queen in the upper-right diagonal
    for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++) {
        if (board[i][j] == 'Q') {
            return false;
        }
    }
    
    return true;
}

private List<String> constructSolution(char[][] board) {
    List<String> solution = new ArrayList<>();
    for (char[] row : board) {
        solution.add(new String(row));
    }
    return solution;
}
```

```java
// Optimized solution using sets to track occupied columns and diagonals
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    
    // Initialize the board with empty spaces
    for (int i = 0; i < n; i++) {
        Arrays.fill(board[i], '.');
    }
    
    // Sets to keep track of occupied columns and diagonals
    Set<Integer> cols = new HashSet<>();
    Set<Integer> diagonals = new HashSet<>(); // row - col
    Set<Integer> antiDiagonals = new HashSet<>(); // row + col
    
    backtrack(board, 0, cols, diagonals, antiDiagonals, result);
    
    return result;
}

private void backtrack(char[][] board, int row, Set<Integer> cols, Set<Integer> diagonals, 
                      Set<Integer> antiDiagonals, List<List<String>> result) {
    if (row == board.length) {
        // Found a valid solution
        result.add(constructSolution(board));
        return;
    }
    
    for (int col = 0; col < board.length; col++) {
        int diag = row - col;
        int antiDiag = row + col;
        
        // Check if the position is valid
        if (cols.contains(col) || diagonals.contains(diag) || antiDiagonals.contains(antiDiag)) {
            continue;
        }
        
        // Place the queen
        board[row][col] = 'Q';
        cols.add(col);
        diagonals.add(diag);
        antiDiagonals.add(antiDiag);
        
        // Recursively place queens in the next rows
        backtrack(board, row + 1, cols, diagonals, antiDiagonals, result);
        
        // Backtrack
        board[row][col] = '.';
        cols.remove(col);
        diagonals.remove(diag);
        antiDiagonals.remove(antiDiag);
    }
}

private List<String> constructSolution(char[][] board) {
    List<String> solution = new ArrayList<>();
    for (char[] row : board) {
        solution.add(new String(row));
    }
    return solution;
}
```

```java
// Alternative solution using bit manipulation for even more efficiency
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    
    // Initialize the board with empty spaces
    for (int i = 0; i < n; i++) {
        Arrays.fill(board[i], '.');
    }
    
    backtrack(board, 0, 0, 0, 0, result);
    
    return result;
}

private void backtrack(char[][] board, int row, int cols, int diagonals, int antiDiagonals, 
                      List<List<String>> result) {
    int n = board.length;
    
    if (row == n) {
        // Found a valid solution
        result.add(constructSolution(board));
        return;
    }
    
    // Get positions where queens can be placed in the current row
    int validPositions = ((1 << n) - 1) & ~(cols | diagonals | antiDiagonals);
    
    while (validPositions != 0) {
        // Get the rightmost valid position
        int position = validPositions & -validPositions;
        validPositions -= position;
        
        // Convert the position to a column index
        int col = Integer.numberOfTrailingZeros(position);
        
        // Place the queen
        board[row][col] = 'Q';
        
        // Recursively place queens in the next rows
        backtrack(board, row + 1, 
                 cols | position, 
                 (diagonals | position) << 1, 
                 (antiDiagonals | position) >> 1, 
                 result);
        
        // Backtrack
        board[row][col] = '.';
    }
}

private List<String> constructSolution(char[][] board) {
    List<String> solution = new ArrayList<>();
    for (char[] row : board) {
        solution.add(new String(row));
    }
    return solution;
}
``` 