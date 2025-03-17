# Minimum Window Substring

**Difficulty**: Hard  
**Tags**: String, Hash Table, Sliding Window, Two Pointers

## Question
Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

## Example Input/Output
**Input**: s = "ADOBECODEBANC", t = "ABC"  
**Output**: "BANC"  
**Explanation**: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Input**: s = "a", t = "a"  
**Output**: "a"  
**Explanation**: The entire string s is the minimum window.

**Input**: s = "a", t = "aa"  
**Output**: ""  
**Explanation**: Both 'a's from t must be included in the window. Since the largest window of s only has one 'a', return empty string.

## Solution Algorithm
We can solve this problem using the sliding window technique:

1. Create two hash maps (or arrays) to keep track of:
   - The characters needed from string t and their frequencies
   - The characters currently in our window
2. Use two pointers to maintain a sliding window:
   - Right pointer expands the window by adding characters
   - Left pointer contracts the window when possible
3. Track the number of characters from t that we've found in our current window
4. When we find a valid window (containing all characters from t), try to minimize it by moving the left pointer
5. Keep track of the minimum window found so far

## Solution Code
```java
public String minWindow(String s, String t) {
    if (s == null || t == null || s.length() == 0 || t.length() == 0 || s.length() < t.length()) {
        return "";
    }
    
    // Create frequency maps
    int[] targetFreq = new int[128]; // Assuming ASCII characters
    int[] windowFreq = new int[128];
    
    // Fill target frequency map
    for (char c : t.toCharArray()) {
        targetFreq[c]++;
    }
    
    int left = 0;
    int right = 0;
    int requiredChars = t.length(); // Total characters we need to find
    int formedChars = 0; // Characters we've found so far
    
    // To store the result
    int minLen = Integer.MAX_VALUE;
    int minLeft = 0;
    
    while (right < s.length()) {
        // Expand the window by adding the right character
        char rightChar = s.charAt(right);
        windowFreq[rightChar]++;
        
        // If we found a required character, increment the counter
        if (targetFreq[rightChar] > 0 && windowFreq[rightChar] <= targetFreq[rightChar]) {
            formedChars++;
        }
        
        // Try to contract the window from the left
        while (formedChars == requiredChars && left <= right) {
            // Update the minimum window if we found a smaller one
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minLeft = left;
            }
            
            // Remove the leftmost character
            char leftChar = s.charAt(left);
            windowFreq[leftChar]--;
            
            // If we removed a required character, decrement the counter
            if (targetFreq[leftChar] > 0 && windowFreq[leftChar] < targetFreq[leftChar]) {
                formedChars--;
            }
            
            // Contract the window
            left++;
        }
        
        // Expand the window
        right++;
    }
    
    return minLen == Integer.MAX_VALUE ? "" : s.substring(minLeft, minLeft + minLen);
}
```

// Alternative implementation with more optimizations
```java
public String minWindow(String s, String t) {
    if (s == null || t == null || s.length() == 0 || t.length() == 0 || s.length() < t.length()) {
        return "";
    }
    
    // Create frequency maps
    Map<Character, Integer> targetMap = new HashMap<>();
    Map<Character, Integer> windowMap = new HashMap<>();
    
    // Fill target frequency map
    for (char c : t.toCharArray()) {
        targetMap.put(c, targetMap.getOrDefault(c, 0) + 1);
    }
    
    int left = 0;
    int right = 0;
    int required = targetMap.size(); // Number of unique characters we need to find
    int formed = 0; // Number of unique characters we've found with correct frequencies
    
    // To store the result
    int minLen = Integer.MAX_VALUE;
    int minLeft = 0;
    
    while (right < s.length()) {
        // Expand the window by adding the right character
        char rightChar = s.charAt(right);
        windowMap.put(rightChar, windowMap.getOrDefault(rightChar, 0) + 1);
        
        // If we found a required character with correct frequency, increment the counter
        if (targetMap.containsKey(rightChar) && windowMap.get(rightChar).equals(targetMap.get(rightChar))) {
            formed++;
        }
        
        // Try to contract the window from the left
        while (formed == required && left <= right) {
            // Update the minimum window if we found a smaller one
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minLeft = left;
            }
            
            // Remove the leftmost character
            char leftChar = s.charAt(left);
            windowMap.put(leftChar, windowMap.get(leftChar) - 1);
            
            // If we removed a required character with correct frequency, decrement the counter
            if (targetMap.containsKey(leftChar) && windowMap.get(leftChar) < targetMap.get(leftChar)) {
                formed--;
            }
            
            // Contract the window
            left++;
        }
        
        // Expand the window
        right++;
    }
    
    return minLen == Integer.MAX_VALUE ? "" : s.substring(minLeft, minLeft + minLen);
}
``` 