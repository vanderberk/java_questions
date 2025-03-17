# Run-Length Encoding

**Difficulty**: Easy  
**Tags**: String, Compression

## Question
Implement run-length encoding (RLE), a simple form of data compression. For a string with consecutive repeated characters, RLE replaces the repeated characters with a count and the character itself.

For example, the string "AAABBBCCDAA" would be encoded as "3A3B2C1D2A".

Write a function that takes a string as input and returns its run-length encoded version.

## Example Input/Output
**Input**: "AAABBBCCDAA"  
**Output**: "3A3B2C1D2A"

**Input**: "ABCDEFG"  
**Output**: "1A1B1C1D1E1F1G"

**Input**: ""  
**Output**: ""

## Solution Algorithm
The algorithm for run-length encoding is straightforward:

1. Initialize an empty result string.
2. If the input string is empty, return the empty result.
3. Initialize a counter to 1 for the first character.
4. Iterate through the string starting from the second character:
   - If the current character is the same as the previous one, increment the counter.
   - If the current character is different from the previous one, append the counter and the previous character to the result, then reset the counter to 1.
5. After the loop, append the counter and the last character to the result.
6. Return the result.

## Solution Code
```java
public String runLengthEncode(String input) {
    if (input == null || input.isEmpty()) {
        return "";
    }
    
    StringBuilder result = new StringBuilder();
    char currentChar = input.charAt(0);
    int count = 1;
    
    for (int i = 1; i < input.length(); i++) {
        if (input.charAt(i) == currentChar) {
            count++;
        } else {
            result.append(count).append(currentChar);
            currentChar = input.charAt(i);
            count = 1;
        }
    }
    
    // Append the last character and its count
    result.append(count).append(currentChar);
    
    return result.toString();
}
```

```java
// Alternative implementation with more detailed comments
public String runLengthEncode(String input) {
    // Handle edge cases
    if (input == null || input.isEmpty()) {
        return "";
    }
    
    StringBuilder encoded = new StringBuilder();
    
    // Initialize with the first character
    char prevChar = input.charAt(0);
    int count = 1;
    
    // Process the rest of the string
    for (int i = 1; i < input.length(); i++) {
        char currentChar = input.charAt(i);
        
        // If the current character is the same as the previous one
        if (currentChar == prevChar) {
            count++;
        } else {
            // Append the count and character to the result
            encoded.append(count).append(prevChar);
            
            // Reset for the new character
            prevChar = currentChar;
            count = 1;
        }
    }
    
    // Don't forget to append the last sequence
    encoded.append(count).append(prevChar);
    
    return encoded.toString();
}
```

```java
// Implementation with a limit on the maximum count (for formats that limit count to 9)
public String runLengthEncode(String input) {
    if (input == null || input.isEmpty()) {
        return "";
    }
    
    StringBuilder result = new StringBuilder();
    char currentChar = input.charAt(0);
    int count = 1;
    int maxCount = 9; // Maximum count before starting a new group
    
    for (int i = 1; i < input.length(); i++) {
        if (input.charAt(i) == currentChar && count < maxCount) {
            count++;
        } else {
            // Append the current count and character
            result.append(count).append(currentChar);
            
            if (input.charAt(i) == currentChar) {
                // If we reached max count but character is the same, start a new group
                currentChar = input.charAt(i);
                count = 1;
            } else {
                // Character changed, reset counter
                currentChar = input.charAt(i);
                count = 1;
            }
        }
    }
    
    // Append the last character and its count
    result.append(count).append(currentChar);
    
    return result.toString();
}
``` 