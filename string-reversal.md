# String Reversal

**Difficulty**: Easy  
**Tags**: String, Two Pointers

## Question
Write a Java method that reverses a given string. Implement the reverse method yourself without using any built-in reverse methods.

### Requirements:
1. Do not use any built-in reverse methods
2. Handle empty strings and strings with a single character
3. Preserve the case of letters
4. The solution should be efficient in terms of both time and space complexity

## Example Input/Output
**Input**: "hello"  
**Output**: "olleh"

**Input**: "Java Programming"  
**Output**: "gnimmargorP avaJ"

**Input**: ""  
**Output**: ""

**Input**: "a"  
**Output**: "a"

## Solution Algorithm
1. Convert the string to a character array
2. Initialize two pointers: left at the start and right at the end
3. While left pointer is less than right pointer:
   - Swap characters at left and right positions
   - Move left pointer forward and right pointer backward
4. Convert the character array back to string and return it

## Solution Code
```java
public class StringReversal {
    public String reverse(String str) {
        if (str == null || str.length() <= 1) {
            return str;
        }
        
        char[] chars = str.toCharArray();
        int left = 0;
        int right = chars.length - 1;
        
        while (left < right) {
            char temp = chars[left];
            chars[left] = chars[right];
            chars[right] = temp;
            
            left++;
            right--;
        }
        
        return new String(chars);
    }
}
```

## Alternative Solutions

1. Using StringBuilder:
```java
public String reverseUsingBuilder(String str) {
    if (str == null || str.length() <= 1) {
        return str;
    }
    
    StringBuilder reversed = new StringBuilder();
    for (int i = str.length() - 1; i >= 0; i--) {
        reversed.append(str.charAt(i));
    }
    
    return reversed.toString();
}
```

2. Using Recursion:
```java
public String reverseRecursive(String str) {
    if (str == null || str.length() <= 1) {
        return str;
    }
    return reverseRecursive(str.substring(1)) + str.charAt(0);
}
```

## Time and Space Complexity
- Two Pointers Approach:
  - Time Complexity: O(n)
  - Space Complexity: O(n)
- StringBuilder Approach:
  - Time Complexity: O(n)
  - Space Complexity: O(n)
- Recursive Approach:
  - Time Complexity: O(n²)
  - Space Complexity: O(n) 