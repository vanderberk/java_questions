# Implement strStr()

**Difficulty**: Easy  
**Tags**: String, Two Pointers, String Matching

## Question
Implement the `strStr()` function.

Given two strings `haystack` and `needle`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Clarification**:
- What should we return when `needle` is an empty string? This is a great question to ask during an interview.
- For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent with C's `strstr()` and Java's `indexOf()`.

## Example Input/Output
**Input**: haystack = "hello", needle = "ll"  
**Output**: 2

**Input**: haystack = "aaaaa", needle = "bba"  
**Output**: -1

**Input**: haystack = "", needle = ""  
**Output**: 0

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Brute Force
1. Iterate through the haystack string
2. For each position, check if the substring starting at that position matches the needle
3. If a match is found, return the starting index
4. If no match is found after checking all positions, return -1

### Approach 2: KMP Algorithm (Knuth-Morris-Pratt)
This is a more efficient algorithm for string matching, but it's more complex:
1. Preprocess the needle to compute a partial match table (also called the "failure function")
2. Use this table to skip unnecessary comparisons when a mismatch occurs
3. This reduces the time complexity from O((n-m+1)*m) to O(n+m)

For an easy-level problem, the brute force approach is usually sufficient.

## Solution Code
```java
// Approach 1: Brute Force
public int strStr(String haystack, String needle) {
    // Edge cases
    if (needle.isEmpty()) {
        return 0;
    }
    
    int haystackLength = haystack.length();
    int needleLength = needle.length();
    
    // If needle is longer than haystack, it can't be found
    if (needleLength > haystackLength) {
        return -1;
    }
    
    // Check each potential starting position in haystack
    for (int i = 0; i <= haystackLength - needleLength; i++) {
        int j;
        
        // Check if substring starting at position i matches needle
        for (j = 0; j < needleLength; j++) {
            if (haystack.charAt(i + j) != needle.charAt(j)) {
                break;
            }
        }
        
        // If we've checked all characters in needle and they all match
        if (j == needleLength) {
            return i;
        }
    }
    
    return -1;
}
```

```java
// Approach 2: Using Java's built-in indexOf method
public int strStr(String haystack, String needle) {
    return haystack.indexOf(needle);
}
```

```java
// Approach 3: KMP Algorithm (more advanced)
public int strStr(String haystack, String needle) {
    if (needle.isEmpty()) {
        return 0;
    }
    
    int m = needle.length();
    int n = haystack.length();
    
    if (n < m) {
        return -1;
    }
    
    // Compute the partial match table (failure function)
    int[] lps = computeLPSArray(needle);
    
    int i = 0; // Index for haystack
    int j = 0; // Index for needle
    
    while (i < n) {
        // Current characters match, move both pointers
        if (needle.charAt(j) == haystack.charAt(i)) {
            i++;
            j++;
        }
        
        // Found a match
        if (j == m) {
            return i - j;
        } 
        // Mismatch after j matches
        else if (i < n && needle.charAt(j) != haystack.charAt(i)) {
            // Use the partial match table to skip characters
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    
    return -1;
}

// Helper function to compute the Longest Proper Prefix which is also Suffix array
private int[] computeLPSArray(String pattern) {
    int m = pattern.length();
    int[] lps = new int[m];
    
    int len = 0;
    int i = 1;
    
    while (i < m) {
        if (pattern.charAt(i) == pattern.charAt(len)) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    
    return lps;
}
``` 