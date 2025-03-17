# Decode String

**Difficulty**: Medium  
**Tags**: String, Stack, Recursion

## Question
Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

The test cases are generated so that the length of the output will never exceed 10^5.

## Example Input/Output
**Input**: s = "3[a]2[bc]"  
**Output**: "aaabcbc"

**Input**: s = "3[a2[c]]"  
**Output**: "accaccacc"

**Input**: s = "2[abc]3[cd]ef"  
**Output**: "abcabccdcdcdef"

## Solution Algorithm
We can solve this problem using a stack-based approach:

1. Initialize an empty stack.
2. Iterate through each character in the string:
   - If the character is a digit, parse the number (which could be multi-digit).
   - If the character is an opening bracket '[', push the current number and reset the number to 0. Also push the current result string and reset it to an empty string.
   - If the character is a closing bracket ']', pop the last string and the last number from the stack. Repeat the current result string by the number and append it to the popped string.
   - If the character is a letter, append it to the current result string.
3. Return the final result string.

## Solution Code
```java
public String decodeString(String s) {
    Stack<Integer> countStack = new Stack<>();
    Stack<StringBuilder> stringStack = new Stack<>();
    StringBuilder currentString = new StringBuilder();
    int count = 0;
    
    for (char ch : s.toCharArray()) {
        if (Character.isDigit(ch)) {
            count = count * 10 + (ch - '0');
        } else if (ch == '[') {
            // Push the current count and string to their respective stacks
            countStack.push(count);
            stringStack.push(currentString);
            // Reset count and currentString for the next segment
            count = 0;
            currentString = new StringBuilder();
        } else if (ch == ']') {
            // Pop the last string and count
            StringBuilder temp = currentString;
            currentString = stringStack.pop();
            int repeatCount = countStack.pop();
            
            // Repeat the current string and append it to the previous string
            for (int i = 0; i < repeatCount; i++) {
                currentString.append(temp);
            }
        } else {
            // Append the character to the current string
            currentString.append(ch);
        }
    }
    
    return currentString.toString();
}
```

```java
// Alternative recursive solution
public String decodeString(String s) {
    return decode(s, new int[]{0});
}

private String decode(String s, int[] index) {
    StringBuilder result = new StringBuilder();
    
    while (index[0] < s.length() && s.charAt(index[0]) != ']') {
        if (!Character.isDigit(s.charAt(index[0]))) {
            // If it's a character, append it to the result
            result.append(s.charAt(index[0]++));
        } else {
            // If it's a digit, parse the number
            int count = 0;
            while (index[0] < s.length() && Character.isDigit(s.charAt(index[0]))) {
                count = count * 10 + (s.charAt(index[0]++) - '0');
            }
            
            // Skip the opening bracket '['
            index[0]++;
            
            // Recursively decode the string inside the brackets
            String decodedString = decode(s, index);
            
            // Skip the closing bracket ']'
            index[0]++;
            
            // Repeat the decoded string 'count' times
            for (int i = 0; i < count; i++) {
                result.append(decodedString);
            }
        }
    }
    
    return result.toString();
}
``` 