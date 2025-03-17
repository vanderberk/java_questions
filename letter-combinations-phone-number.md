# Letter Combinations of a Phone Number

**Difficulty**: Medium  
**Tags**: String, Backtracking, Recursion, Hash Table

## Question
Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

```
2 -> abc
3 -> def
4 -> ghi
5 -> jkl
6 -> mno
7 -> pqrs
8 -> tuv
9 -> wxyz
```

## Example Input/Output
**Input**: digits = "23"  
**Output**: ["ad","ae","af","bd","be","bf","cd","ce","cf"]

**Input**: digits = ""  
**Output**: []

**Input**: digits = "2"  
**Output**: ["a","b","c"]

## Solution Algorithm
We can solve this problem using backtracking:

1. Create a mapping from digits to their corresponding letters.
2. Use a recursive function to generate all possible combinations:
   - Base case: If we have processed all digits, add the current combination to the result.
   - For each letter corresponding to the current digit, add it to the current combination and recursively process the next digit.
3. Return the list of all combinations.

## Solution Code
```java
public List<String> letterCombinations(String digits) {
    List<String> result = new ArrayList<>();
    
    // Edge case: empty input
    if (digits == null || digits.length() == 0) {
        return result;
    }
    
    // Mapping of digits to letters
    Map<Character, String> digitToLetters = new HashMap<>();
    digitToLetters.put('2', "abc");
    digitToLetters.put('3', "def");
    digitToLetters.put('4', "ghi");
    digitToLetters.put('5', "jkl");
    digitToLetters.put('6', "mno");
    digitToLetters.put('7', "pqrs");
    digitToLetters.put('8', "tuv");
    digitToLetters.put('9', "wxyz");
    
    // Start backtracking
    backtrack(digits, 0, new StringBuilder(), result, digitToLetters);
    
    return result;
}

private void backtrack(String digits, int index, StringBuilder current, List<String> result, Map<Character, String> digitToLetters) {
    // Base case: we've processed all digits
    if (index == digits.length()) {
        result.add(current.toString());
        return;
    }
    
    // Get the letters corresponding to the current digit
    char digit = digits.charAt(index);
    String letters = digitToLetters.get(digit);
    
    // Try each letter
    for (int i = 0; i < letters.length(); i++) {
        // Add the current letter to the combination
        current.append(letters.charAt(i));
        
        // Recursively process the next digit
        backtrack(digits, index + 1, current, result, digitToLetters);
        
        // Backtrack: remove the last letter to try the next one
        current.deleteCharAt(current.length() - 1);
    }
}
```

// Alternative implementation using a queue (BFS approach)
```java
public List<String> letterCombinations(String digits) {
    List<String> result = new ArrayList<>();
    
    // Edge case: empty input
    if (digits == null || digits.length() == 0) {
        return result;
    }
    
    // Mapping of digits to letters
    String[] digitToLetters = new String[] {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    
    // Initialize the queue with an empty string
    Queue<String> queue = new LinkedList<>();
    queue.add("");
    
    // Process each digit
    for (int i = 0; i < digits.length(); i++) {
        int digit = Character.getNumericValue(digits.charAt(i));
        String letters = digitToLetters[digit];
        
        // Process all combinations in the queue
        int size = queue.size();
        for (int j = 0; j < size; j++) {
            String current = queue.poll();
            
            // Add each letter to the current combination
            for (char letter : letters.toCharArray()) {
                queue.add(current + letter);
            }
        }
    }
    
    // The queue now contains all combinations
    result.addAll(queue);
    
    return result;
}
``` 