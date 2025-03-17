# Excel Sheet Column Number

**Difficulty**: Easy  
**Tags**: Math, String

## Question
Given a string `columnTitle` that represents the column title as appears in an Excel sheet, return its corresponding column number.

For example:
```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

## Example Input/Output
**Input**: columnTitle = "A"  
**Output**: 1

**Input**: columnTitle = "AB"  
**Output**: 28

**Input**: columnTitle = "ZY"  
**Output**: 701

**Input**: columnTitle = "FXSHRXW"  
**Output**: 2147483647

## Solution Algorithm
This problem is essentially asking us to convert a string from base-26 to base-10, with a slight twist: the digits are represented by letters A-Z (where A=1, B=2, ..., Z=26) instead of 0-25.

The algorithm is as follows:
1. Initialize the result to 0.
2. Iterate through each character in the string from left to right.
3. For each character:
   - Multiply the current result by 26 (shifting left in base-26).
   - Add the value of the current character (A=1, B=2, ..., Z=26) to the result.
4. Return the final result.

The value of a character can be calculated as `char - 'A' + 1`.

## Solution Code
```java
public int titleToNumber(String columnTitle) {
    int result = 0;
    
    for (int i = 0; i < columnTitle.length(); i++) {
        // Get the decimal value of the character (A=1, B=2, ..., Z=26)
        int charValue = columnTitle.charAt(i) - 'A' + 1;
        
        // Multiply the current result by 26 and add the character value
        result = result * 26 + charValue;
    }
    
    return result;
}
```

```java
// Alternative implementation using enhanced for loop
public int titleToNumber(String columnTitle) {
    int result = 0;
    
    for (char c : columnTitle.toCharArray()) {
        // Get the decimal value of the character (A=1, B=2, ..., Z=26)
        int charValue = c - 'A' + 1;
        
        // Multiply the current result by 26 and add the character value
        result = result * 26 + charValue;
    }
    
    return result;
}
```

```java
// Implementation using Java 8 streams
public int titleToNumber(String columnTitle) {
    return columnTitle.chars()
            .reduce(0, (result, c) -> result * 26 + (c - 'A' + 1));
}
``` 