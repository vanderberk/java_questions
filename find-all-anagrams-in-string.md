# Find All Anagrams in a String

**Difficulty**: Medium  
**Tags**: Hash Table, String, Sliding Window

## Question
Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`. You may return the answer in any order.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Example Input/Output
**Input**: s = "cbaebabacd", p = "abc"  
**Output**: [0,6]  
**Explanation**:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".

**Input**: s = "abab", p = "ab"  
**Output**: [0,1,2]  
**Explanation**:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

## Solution Algorithm
We can solve this problem using a sliding window approach with a frequency counter:

1. Create frequency counters for pattern `p` and the current window in string `s`.
2. Initialize a sliding window of size equal to the length of pattern `p`.
3. For each window position:
   - Update the frequency counter for the current window.
   - Compare the frequency counters of the current window and pattern `p`.
   - If they match, add the start index of the window to the result.
   - Slide the window by one position (remove the leftmost character and add the next character).
4. Return the result array.

## Solution Code
```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();
    
    if (s == null || s.length() == 0 || p == null || p.length() == 0 || s.length() < p.length()) {
        return result;
    }
    
    // Create frequency counters
    int[] pCount = new int[26];
    int[] sCount = new int[26];
    
    // Initialize frequency counter for pattern p
    for (char c : p.toCharArray()) {
        pCount[c - 'a']++;
    }
    
    // Initialize the sliding window
    int windowSize = p.length();
    for (int i = 0; i < windowSize; i++) {
        sCount[s.charAt(i) - 'a']++;
    }
    
    // Check if the initial window is an anagram
    if (Arrays.equals(sCount, pCount)) {
        result.add(0);
    }
    
    // Slide the window
    for (int i = windowSize; i < s.length(); i++) {
        // Add the next character to the window
        sCount[s.charAt(i) - 'a']++;
        
        // Remove the leftmost character from the window
        sCount[s.charAt(i - windowSize) - 'a']--;
        
        // Check if the current window is an anagram
        if (Arrays.equals(sCount, pCount)) {
            result.add(i - windowSize + 1);
        }
    }
    
    return result;
}
```

```java
// Alternative solution using a more efficient approach
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();
    
    if (s == null || s.length() == 0 || p == null || p.length() == 0 || s.length() < p.length()) {
        return result;
    }
    
    // Create frequency counter
    int[] count = new int[26];
    
    // Initialize frequency counter for pattern p
    for (char c : p.toCharArray()) {
        count[c - 'a']++;
    }
    
    int left = 0;
    int right = 0;
    int counter = p.length(); // Number of characters to match
    
    while (right < s.length()) {
        // If the current character exists in p, decrement the counter
        if (count[s.charAt(right) - 'a'] > 0) {
            counter--;
        }
        
        // Decrement the frequency of the current character
        count[s.charAt(right) - 'a']--;
        right++;
        
        // If all characters are matched
        if (counter == 0) {
            result.add(left);
        }
        
        // If the window size equals the pattern length
        if (right - left == p.length()) {
            // If the leftmost character exists in p, increment the counter
            if (count[s.charAt(left) - 'a'] >= 0) {
                counter++;
            }
            
            // Increment the frequency of the leftmost character
            count[s.charAt(left) - 'a']++;
            left++;
        }
    }
    
    return result;
}
``` 