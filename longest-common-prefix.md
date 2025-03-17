# Longest Common Prefix

**Difficulty**: Easy  
**Tags**: String, Trie

## Question
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

## Example Input/Output
**Input**: strs = ["flower","flow","flight"]  
**Output**: "fl"

**Input**: strs = ["dog","racecar","car"]  
**Output**: ""  
**Explanation**: There is no common prefix among the input strings.

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Horizontal Scanning
1. Take the first string as the initial prefix.
2. Iterate through the remaining strings and find the common prefix between the current prefix and each string.
3. Update the prefix after each comparison.
4. If at any point the prefix becomes empty, return an empty string.

### Approach 2: Vertical Scanning
1. Scan the strings character by character.
2. For each position, check if all strings have the same character at that position.
3. If they do, add the character to the result.
4. If they don't, or if we reach the end of any string, return the result.

### Approach 3: Divide and Conquer
1. Divide the array of strings into two halves.
2. Recursively find the common prefix for each half.
3. Find the common prefix of the two results.

### Approach 4: Binary Search
1. Find the minimum length string in the array.
2. Use binary search to find the longest common prefix within the range [0, minLength].

## Solution Code
```java
// Approach 1: Horizontal Scanning
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    
    String prefix = strs[0];
    
    for (int i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) {
                return "";
            }
        }
    }
    
    return prefix;
}
```

```java
// Approach 2: Vertical Scanning
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    
    for (int i = 0; i < strs[0].length(); i++) {
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c) {
                return strs[0].substring(0, i);
            }
        }
    }
    
    return strs[0];
}
```

```java
// Approach 3: Divide and Conquer
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    
    return longestCommonPrefix(strs, 0, strs.length - 1);
}

private String longestCommonPrefix(String[] strs, int left, int right) {
    if (left == right) {
        return strs[left];
    }
    
    int mid = left + (right - left) / 2;
    String lcpLeft = longestCommonPrefix(strs, left, mid);
    String lcpRight = longestCommonPrefix(strs, mid + 1, right);
    
    return commonPrefix(lcpLeft, lcpRight);
}

private String commonPrefix(String left, String right) {
    int min = Math.min(left.length(), right.length());
    
    for (int i = 0; i < min; i++) {
        if (left.charAt(i) != right.charAt(i)) {
            return left.substring(0, i);
        }
    }
    
    return left.substring(0, min);
}
``` 