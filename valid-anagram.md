# Valid Anagram

**Difficulty**: Easy  
**Tags**: Hash Table, String, Sorting

## Question
Given two strings `s` and `t`, return true if `t` is an anagram of `s`, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Example Input/Output
**Input**: s = "anagram", t = "nagaram"  
**Output**: true

**Input**: s = "rat", t = "car"  
**Output**: false

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Sorting
1. Sort both strings.
2. Compare the sorted strings. If they are equal, return true; otherwise, return false.

### Approach 2: Hash Table (Character Count)
1. If the lengths of the two strings are different, return false.
2. Create a hash table (or an array for lowercase English letters) to count the occurrences of each character in the first string.
3. Decrement the count for each character in the second string.
4. If any count becomes negative or if any character in the second string is not in the hash table, return false.
5. Otherwise, return true.

## Solution Code
```java
// Approach 1: Sorting
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    
    char[] sChars = s.toCharArray();
    char[] tChars = t.toCharArray();
    
    Arrays.sort(sChars);
    Arrays.sort(tChars);
    
    return Arrays.equals(sChars, tChars);
}
```

```java
// Approach 2: Hash Table (Character Count)
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    
    int[] count = new int[26]; // Assuming lowercase English letters
    
    // Count characters in s
    for (char c : s.toCharArray()) {
        count[c - 'a']++;
    }
    
    // Decrement count for characters in t
    for (char c : t.toCharArray()) {
        count[c - 'a']--;
        if (count[c - 'a'] < 0) {
            return false; // More occurrences of c in t than in s
        }
    }
    
    return true;
}
```

```java
// Approach 2 (Alternative): Using HashMap for Unicode characters
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    
    Map<Character, Integer> count = new HashMap<>();
    
    // Count characters in s
    for (char c : s.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) + 1);
    }
    
    // Decrement count for characters in t
    for (char c : t.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) - 1);
        if (count.get(c) < 0) {
            return false; // More occurrences of c in t than in s
        }
    }
    
    return true;
}
``` 