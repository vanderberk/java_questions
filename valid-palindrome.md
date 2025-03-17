# Valid Palindrome

**Difficulty**: Easy  
**Tags**: Strings, Two Pointers

## Question
Determine if a given string is a palindrome, considering only alphanumeric characters and ignoring case sensitivity.

## Example Input/Output
**Input**: "A man, a plan, a canal: Panama"  
**Output**: true

**Input**: "race a car"  
**Output**: false

## Solution Algorithm
1. Convert the string to lowercase
2. Use two pointers (left and right) starting from the beginning and end of the string
3. Skip non-alphanumeric characters
4. Compare characters at the left and right pointers
5. If all characters match, the string is a palindrome

## Solution Code
```java
public boolean isPalindrome(String s) {
    if (s == null || s.length() == 0) {
        return true;
    }
    
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        // Skip non-alphanumeric characters from the left
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        
        // Skip non-alphanumeric characters from the right
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        
        // Compare characters (ignoring case)
        if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
            return false;
        }
        
        left++;
        right--;
    }
    
    return true;
}
``` 