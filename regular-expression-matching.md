# Regular Expression Matching

**Difficulty**: Hard  
**Tags**: String, Dynamic Programming, Recursion

## Question
Given an input string s and a pattern p, implement regular expression matching with support for '.' and '*' where:
- '.' Matches any single character.
- '*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

## Example Input/Output
**Input**: s = "aa", p = "a"  
**Output**: false  
**Explanation**: "a" does not match the entire string "aa".

**Input**: s = "aa", p = "a*"  
**Output**: true  
**Explanation**: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

**Input**: s = "ab", p = ".*"  
**Output**: true  
**Explanation**: ".*" means "zero or more (*) of any character (.)".

## Solution Algorithm
1. Use dynamic programming with a 2D boolean array dp
2. dp[i][j] represents whether the first i characters in string s match the first j characters in pattern p
3. Base case: dp[0][0] = true (empty string matches empty pattern)
4. For each character in the pattern:
   - If the current character is '*', we have two scenarios:
     - Zero occurrences of the preceding element: dp[i][j] = dp[i][j-2]
     - One or more occurrences: dp[i][j] = dp[i-1][j] if s[i-1] matches p[j-2] or p[j-2] is '.'
   - If the current character is not '*', we check if the current characters match:
     - dp[i][j] = dp[i-1][j-1] if s[i-1] matches p[j-1] or p[j-1] is '.'
5. Return dp[s.length()][p.length()]

## Solution Code
```java
public boolean isMatch(String s, String p) {
    int m = s.length();
    int n = p.length();
    
    // dp[i][j] represents if the first i characters in s match the first j characters in p
    boolean[][] dp = new boolean[m + 1][n + 1];
    
    // Empty pattern matches empty string
    dp[0][0] = true;
    
    // Handle patterns like a*, a*b*, etc.
    for (int j = 1; j <= n; j++) {
        if (p.charAt(j - 1) == '*') {
            dp[0][j] = dp[0][j - 2];
        }
    }
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            char pc = p.charAt(j - 1);
            char sc = s.charAt(i - 1);
            
            if (pc == '*') {
                // Zero occurrence of the character before '*'
                dp[i][j] = dp[i][j - 2];
                
                // One or more occurrences of the character before '*'
                if (p.charAt(j - 2) == '.' || p.charAt(j - 2) == sc) {
                    dp[i][j] = dp[i][j] || dp[i - 1][j];
                }
            } else if (pc == '.' || pc == sc) {
                // Current characters match
                dp[i][j] = dp[i - 1][j - 1];
            }
        }
    }
    
    return dp[m][n];
}
``` 