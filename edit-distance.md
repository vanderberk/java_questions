# Edit Distance

**Difficulty**: Hard  
**Tags**: String, Dynamic Programming

## Question
Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.

You have the following three operations permitted on a word:
- Insert a character
- Delete a character
- Replace a character

## Example Input/Output
**Input**: word1 = "horse", word2 = "ros"  
**Output**: 3  
**Explanation**:
- horse -> rorse (replace 'h' with 'r')
- rorse -> rose (remove 'r')
- rose -> ros (remove 'e')

**Input**: word1 = "intention", word2 = "execution"  
**Output**: 5  
**Explanation**:
- intention -> inention (remove 't')
- inention -> enention (replace 'i' with 'e')
- enention -> exention (replace 'n' with 'x')
- exention -> exection (replace 'n' with 'c')
- exection -> execution (insert 'u')

## Solution Algorithm
This problem can be solved using dynamic programming. We define a 2D array `dp` where `dp[i][j]` represents the minimum number of operations required to convert the first `i` characters of `word1` to the first `j` characters of `word2`.

### Base Cases:
- `dp[i][0]` = `i` (It takes `i` deletions to convert a string of length `i` to an empty string)
- `dp[0][j]` = `j` (It takes `j` insertions to convert an empty string to a string of length `j`)

### Recurrence Relation:
- If `word1[i-1]` equals `word2[j-1]`, then `dp[i][j] = dp[i-1][j-1]` (No operation needed for the current characters)
- Otherwise, `dp[i][j]` is the minimum of:
  - `dp[i-1][j] + 1` (Delete the current character from `word1`)
  - `dp[i][j-1] + 1` (Insert the current character of `word2` into `word1`)
  - `dp[i-1][j-1] + 1` (Replace the current character of `word1` with the current character of `word2`)

### Time Complexity: O(m * n), where m and n are the lengths of the two strings.
### Space Complexity: O(m * n) for the DP table.

## Solution Code
```java
public int minDistance(String word1, String word2) {
    int m = word1.length();
    int n = word2.length();
    
    // Create a DP table
    int[][] dp = new int[m + 1][n + 1];
    
    // Base cases
    for (int i = 0; i <= m; i++) {
        dp[i][0] = i; // Convert word1[0...i-1] to empty string
    }
    
    for (int j = 0; j <= n; j++) {
        dp[0][j] = j; // Convert empty string to word2[0...j-1]
    }
    
    // Fill the DP table
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1]; // No operation needed
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i - 1][j],     // Delete
                    Math.min(
                        dp[i][j - 1],     // Insert
                        dp[i - 1][j - 1]  // Replace
                    )
                );
            }
        }
    }
    
    return dp[m][n];
}
```

```java
// Space-optimized solution using only O(min(m, n)) space
public int minDistance(String word1, String word2) {
    // Ensure word1 is the shorter string for space optimization
    if (word1.length() > word2.length()) {
        String temp = word1;
        word1 = word2;
        word2 = temp;
    }
    
    int m = word1.length();
    int n = word2.length();
    
    // Create a 1D DP array
    int[] dp = new int[m + 1];
    
    // Base case: converting word1[0...i-1] to empty string
    for (int i = 0; i <= m; i++) {
        dp[i] = i;
    }
    
    // Fill the DP array
    for (int j = 1; j <= n; j++) {
        int prev = dp[0]; // Store dp[i-1][j-1]
        dp[0] = j; // Base case: converting empty string to word2[0...j-1]
        
        for (int i = 1; i <= m; i++) {
            int temp = dp[i]; // Store the current value before updating
            
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i] = prev; // No operation needed
            } else {
                dp[i] = 1 + Math.min(
                    dp[i - 1],  // Delete
                    Math.min(
                        dp[i],      // Insert (which is the previous dp[i][j-1])
                        prev        // Replace (which is the previous dp[i-1][j-1])
                    )
                );
            }
            
            prev = temp; // Update prev for the next iteration
        }
    }
    
    return dp[m];
}
```

```java
// Recursive solution with memoization
public int minDistance(String word1, String word2) {
    int m = word1.length();
    int n = word2.length();
    
    // Create a memoization table
    Integer[][] memo = new Integer[m + 1][n + 1];
    
    return minDistanceHelper(word1, word2, m, n, memo);
}

private int minDistanceHelper(String word1, String word2, int i, int j, Integer[][] memo) {
    // Base cases
    if (i == 0) {
        return j; // Insert j characters
    }
    
    if (j == 0) {
        return i; // Delete i characters
    }
    
    // Check if the result is already memoized
    if (memo[i][j] != null) {
        return memo[i][j];
    }
    
    // If the current characters are the same, no operation needed
    if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
        memo[i][j] = minDistanceHelper(word1, word2, i - 1, j - 1, memo);
        return memo[i][j];
    }
    
    // Calculate the minimum of the three operations
    int delete = minDistanceHelper(word1, word2, i - 1, j, memo);
    int insert = minDistanceHelper(word1, word2, i, j - 1, memo);
    int replace = minDistanceHelper(word1, word2, i - 1, j - 1, memo);
    
    memo[i][j] = 1 + Math.min(delete, Math.min(insert, replace));
    return memo[i][j];
}
``` 