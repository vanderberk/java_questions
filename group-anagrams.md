# Group Anagrams

**Difficulty**: Medium  
**Tags**: Hash Table, String, Sorting

## Question
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Example Input/Output
**Input**: strs = ["eat","tea","tan","ate","nat","bat"]  
**Output**: [["bat"],["nat","tan"],["ate","eat","tea"]]

**Input**: strs = [""]  
**Output**: [[""]]

**Input**: strs = ["a"]  
**Output**: [["a"]]

## Solution Algorithm
1. Create a HashMap where:
   - The key is a string representation of the sorted characters of each word
   - The value is a list of original words that, when sorted, match the key
2. Iterate through each string in the input array:
   - Sort the characters of the string to create a key
   - Add the original string to the list associated with that key in the HashMap
3. Return the values of the HashMap as a list of lists

## Solution Code
```java
public List<List<String>> groupAnagrams(String[] strs) {
    if (strs == null || strs.length == 0) {
        return new ArrayList<>();
    }
    
    Map<String, List<String>> anagramGroups = new HashMap<>();
    
    for (String s : strs) {
        // Convert string to char array and sort it to get a unique key for anagrams
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        String key = String.valueOf(chars);
        
        // Add the original string to its anagram group
        if (!anagramGroups.containsKey(key)) {
            anagramGroups.put(key, new ArrayList<>());
        }
        anagramGroups.get(key).add(s);
    }
    
    // Return all the anagram groups
    return new ArrayList<>(anagramGroups.values());
}
```

// Alternative solution using character count as key
```java
public List<List<String>> groupAnagrams(String[] strs) {
    if (strs == null || strs.length == 0) {
        return new ArrayList<>();
    }
    
    Map<String, List<String>> anagramGroups = new HashMap<>();
    
    for (String s : strs) {
        // Create a count of each character (assuming lowercase English letters)
        int[] count = new int[26];
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }
        
        // Build a string key using the character counts
        StringBuilder keyBuilder = new StringBuilder();
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                keyBuilder.append((char)('a' + i)).append(count[i]);
            }
        }
        String key = keyBuilder.toString();
        
        // Add the original string to its anagram group
        if (!anagramGroups.containsKey(key)) {
            anagramGroups.put(key, new ArrayList<>());
        }
        anagramGroups.get(key).add(s);
    }
    
    // Return all the anagram groups
    return new ArrayList<>(anagramGroups.values());
}
``` 