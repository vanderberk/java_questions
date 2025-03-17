# Longest Palindromic Substring

**Difficulty**: Medium  
**Tags**: Strings, Dynamic Programming

## Question
Given a string s, return the longest palindromic substring in s.

## Example Input/Output
**Input**: "babad"  
**Output**: "bab" or "aba" (both are valid)

**Input**: "cbbd"  
**Output**: "bb"

## Solution Algorithm
1. Use the expand around center approach
2. For each character in the string:
   - Consider it as the center of a palindrome
   - Expand outward to find the longest palindrome centered at this position
   - Consider both odd-length palindromes (single character center) and even-length palindromes (between two characters)
3. Keep track of the longest palindrome found
4. Return the longest palindromic substring

## Solution Code
```java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 2) {
        return s;
    }
    
    int start = 0;
    int maxLength = 1;
    
    for (int i = 0; i < s.length(); i++) {
        // Expand for odd-length palindromes
        expandAroundCenter(s, i, i, start, maxLength);
        
        // Expand for even-length palindromes
        expandAroundCenter(s, i, i + 1, start, maxLength);
    }
    
    return s.substring(start, start + maxLength);
}

private void expandAroundCenter(String s, int left, int right, int start, int maxLength) {
    // Expand outward as long as characters match
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        int currentLength = right - left + 1;
        
        if (currentLength > maxLength) {
            maxLength = currentLength;
            start = left;
        }
        
        left--;
        right++;
    }
}
``` 