# First Unique Character

**Difficulty**: Easy  
**Tags**: Strings, Hash Table, Counting

## Question
Given a string, find the first non-repeating character and return its index. If it doesn't exist, return -1.

## Example Input/Output
**Input**: "leetcode"  
**Output**: 0  
**Explanation**: The first non-repeating character is 'l', which appears at index 0.

**Input**: "loveleetcode"  
**Output**: 2  
**Explanation**: The first non-repeating character is 'v', which appears at index 2.

## Solution Algorithm
1. Create a frequency counter for each character in the string
2. Iterate through the string once to count the frequency of each character
3. Iterate through the string again to find the first character with a frequency of 1
4. Return the index of that character, or -1 if no such character exists

## Solution Code
```java
public int firstUniqChar(String s) {
    if (s == null || s.length() == 0) {
        return -1;
    }
    
    // Create frequency counter
    int[] frequency = new int[26]; // Assuming lowercase English letters
    
    // Count frequency of each character
    for (char c : s.toCharArray()) {
        frequency[c - 'a']++;
    }
    
    // Find the first character with frequency 1
    for (int i = 0; i < s.length(); i++) {
        if (frequency[s.charAt(i) - 'a'] == 1) {
            return i;
        }
    }
    
    return -1; // No unique character found
}
``` 