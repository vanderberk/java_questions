# Unique Paths

**Difficulty**: Medium  
**Tags**: Array, Dynamic Programming, Math, Combinatorics

## Question
A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

## Example Input/Output
**Input**: m = 3, n = 7  
**Output**: 28

**Input**: m = 3, n = 2  
**Output**: 3  
**Explanation**:  
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Right -> Down
3. Down -> Down -> Right

## Solution Algorithm
We can solve this problem using dynamic programming:

### Approach 1: 2D Dynamic Programming
1. Create a 2D array `dp` where `dp[i][j]` represents the number of unique paths to reach the cell at row `i` and column `j`.
2. Initialize the first row and first column of `dp` to 1, as there is only one way to reach any cell in the first row or first column (by moving only right or only down).
3. For each cell `(i, j)` where `i > 0` and `j > 0`, the number of unique paths to reach it is the sum of the number of unique paths to reach the cell above it and the cell to its left: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.
4. Return `dp[m-1][n-1]`.

### Approach 2: 1D Dynamic Programming (Space Optimization)
1. Create a 1D array `dp` of size `n` where `dp[j]` represents the number of unique paths to reach the cell at column `j` in the current row.
2. Initialize all elements of `dp` to 1, as there is only one way to reach any cell in the first row.
3. For each row `i` from 1 to `m-1`:
   - For each column `j` from 1 to `n-1`:
     - Update `dp[j] = dp[j] + dp[j-1]`, where `dp[j]` is the number of paths to reach the cell above, and `dp[j-1]` is the number of paths to reach the cell to the left.
4. Return `dp[n-1]`.

### Approach 3: Mathematical Combination
1. To reach the bottom-right corner, the robot needs to move exactly `m-1` steps down and `n-1` steps right, for a total of `(m-1) + (n-1) = m+n-2` steps.
2. The number of unique paths is the number of ways to choose which of these `m+n-2` steps are down steps (or equivalently, right steps).
3. This is a combination problem: C(m+n-2, m-1) = (m+n-2)! / ((m-1)! * (n-1)!).

## Solution Code
```java
// Approach 1: 2D Dynamic Programming
public int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    
    // Initialize the first row and first column to 1
    for (int i = 0; i < m; i++) {
        dp[i][0] = 1;
    }
    
    for (int j = 0; j < n; j++) {
        dp[0][j] = 1;
    }
    
    // Fill the dp table
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    
    return dp[m-1][n-1];
}
```

```java
// Approach 2: 1D Dynamic Programming (Space Optimization)
public int uniquePaths(int m, int n) {
    int[] dp = new int[n];
    
    // Initialize the dp array
    Arrays.fill(dp, 1);
    
    // Fill the dp array
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[j] += dp[j-1];
        }
    }
    
    return dp[n-1];
}
```

```java
// Approach 3: Mathematical Combination
public int uniquePaths(int m, int n) {
    // We need to calculate C(m+n-2, m-1) = (m+n-2)! / ((m-1)! * (n-1)!)
    
    // To avoid overflow, we can calculate this as:
    // C(m+n-2, m-1) = (m+n-2) * (m+n-3) * ... * (n) / (1 * 2 * ... * (m-1))
    
    long result = 1;
    
    // Choose the smaller of m-1 and n-1 to minimize the number of operations
    int k = Math.min(m - 1, n - 1);
    int j = Math.max(m - 1, n - 1);
    
    // Calculate C(m+n-2, k)
    for (int i = 1; i <= k; i++) {
        result = result * (j + i) / i;
    }
    
    return (int) result;
}
``` 