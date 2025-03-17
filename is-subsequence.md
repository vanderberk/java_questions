# Is Subsequence

**Difficulty**: Easy  
**Tags**: Two Pointers, String, Dynamic Programming

## Question
Given two strings `s` and `t`, return `true` if `s` is a subsequence of `t`, or `false` otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

## Example Input/Output
**Input**: s = "abc", t = "ahbgdc"  
**Output**: true

**Input**: s = "axc", t = "ahbgdc"  
**Output**: false

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Two Pointers
1. Initialize two pointers, one for string `s` and one for string `t`.
2. Iterate through string `t` and compare characters with string `s`.
3. If a match is found, move the pointer for string `s` forward.
4. After the iteration, if the pointer for string `s` has reached the end, return true.
5. Otherwise, return false.

### Approach 2: Recursive Approach
1. Define a recursive function that takes the current indices of both strings.
2. If the current index of string `s` has reached the end, return true.
3. If the current index of string `t` has reached the end, return false.
4. If the characters at the current indices match, recursively call the function with both indices incremented.
5. Otherwise, recursively call the function with only the index of string `t` incremented.

### Approach 3: Dynamic Programming (for follow-up)
1. Create a 2D DP array where dp[i][j] represents whether s[0...i-1] is a subsequence of t[0...j-1].
2. Initialize the base cases: dp[0][j] = true for all j (empty string is a subsequence of any string).
3. Fill the DP array based on the recurrence relation.
4. Return dp[s.length()][t.length()].

## Solution Code
```java
// Approach 1: Two Pointers
public boolean isSubsequence(String s, String t) {
    if (s.length() == 0) {
        return true;
    }
    
    int sIndex = 0;
    int tIndex = 0;
    
    while (tIndex < t.length()) {
        if (s.charAt(sIndex) == t.charAt(tIndex)) {
            sIndex++;
            if (sIndex == s.length()) {
                return true;
            }
        }
        tIndex++;
    }
    
    return false;
}
```

```java
// Approach 2: Recursive Approach
public boolean isSubsequence(String s, String t) {
    return isSubsequenceHelper(s, t, 0, 0);
}

private boolean isSubsequenceHelper(String s, String t, int sIndex, int tIndex) {
    // Base cases
    if (sIndex == s.length()) {
        return true;
    }
    if (tIndex == t.length()) {
        return false;
    }
    
    // If characters match, move both pointers
    if (s.charAt(sIndex) == t.charAt(tIndex)) {
        return isSubsequenceHelper(s, t, sIndex + 1, tIndex + 1);
    }
    
    // If characters don't match, only move t's pointer
    return isSubsequenceHelper(s, t, sIndex, tIndex + 1);
}
```

```java
// Approach 3: Dynamic Programming (for follow-up with multiple queries)
public boolean isSubsequence(String s, String t) {
    int m = s.length();
    int n = t.length();
    
    // Create a 2D DP array
    boolean[][] dp = new boolean[m + 1][n + 1];
    
    // Empty string is a subsequence of any string
    for (int j = 0; j <= n; j++) {
        dp[0][j] = true;
    }
    
    // Fill the DP array
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s.charAt(i - 1) == t.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = dp[i][j - 1];
            }
        }
    }
    
    return dp[m][n];
}
``` 