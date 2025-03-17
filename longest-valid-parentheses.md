# Longest Valid Parentheses

**Difficulty**: Hard  
**Tags**: String, Dynamic Programming, Stack

## Question
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

## Example Input/Output
**Input**: s = "(()"  
**Output**: 2  
**Explanation**: The longest valid parentheses substring is "()".

**Input**: s = ")()())"  
**Output**: 4  
**Explanation**: The longest valid parentheses substring is "()()".

**Input**: s = ""  
**Output**: 0

## Solution Algorithm
This problem can be solved using several approaches:

### Approach 1: Using Stack
1. Initialize a stack with -1 (representing the base index before any valid substring).
2. Iterate through the string:
   - If the current character is '(', push its index onto the stack.
   - If the current character is ')':
     - Pop the top element from the stack.
     - If the stack is empty, push the current index onto the stack (new base).
     - If the stack is not empty, calculate the length of the current valid substring as the difference between the current index and the top of the stack.
3. Keep track of the maximum length found.

### Approach 2: Dynamic Programming
1. Create a DP array where dp[i] represents the length of the longest valid substring ending at index i.
2. Initialize dp[i] = 0 for all i.
3. For each character at index i:
   - If s[i] is '(', dp[i] = 0 (a valid substring cannot end with an opening parenthesis).
   - If s[i] is ')':
     - If s[i-1] is '(', dp[i] = dp[i-2] + 2 (matching a pair directly).
     - If s[i-1] is ')' and s[i-dp[i-1]-1] is '(', dp[i] = dp[i-1] + 2 + dp[i-dp[i-1]-2] (matching a nested pair).
4. Return the maximum value in the DP array.

### Approach 3: Two-pass Counting
1. Initialize left and right counters to 0.
2. Scan from left to right:
   - Increment left when encountering '('.
   - Increment right when encountering ')'.
   - If left equals right, update the maximum length.
   - If right becomes greater than left, reset both counters to 0.
3. Reset counters and scan from right to left (to handle cases like "(()" where left is always greater than right).
4. Return the maximum length found.

## Solution Code
```java
// Approach 1: Using Stack
public int longestValidParentheses(String s) {
    int maxLength = 0;
    Stack<Integer> stack = new Stack<>();
    stack.push(-1); // Base index
    
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        
        if (c == '(') {
            stack.push(i);
        } else { // c == ')'
            stack.pop();
            
            if (stack.isEmpty()) {
                stack.push(i); // New base index
            } else {
                maxLength = Math.max(maxLength, i - stack.peek());
            }
        }
    }
    
    return maxLength;
}
```

```java
// Approach 2: Dynamic Programming
public int longestValidParentheses(String s) {
    int maxLength = 0;
    int[] dp = new int[s.length()];
    
    for (int i = 1; i < s.length(); i++) {
        if (s.charAt(i) == ')') {
            if (s.charAt(i - 1) == '(') {
                // Case: "....()"
                dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
            } else if (i - dp[i - 1] > 0 && s.charAt(i - dp[i - 1] - 1) == '(') {
                // Case: "....))": Check if there's a matching '(' for the current ')'
                dp[i] = dp[i - 1] + 2;
                
                // Add the valid substring before the matching '('
                if (i - dp[i - 1] >= 2) {
                    dp[i] += dp[i - dp[i - 1] - 2];
                }
            }
            
            maxLength = Math.max(maxLength, dp[i]);
        }
    }
    
    return maxLength;
}
```

```java
// Approach 3: Two-pass Counting
public int longestValidParentheses(String s) {
    int maxLength = 0;
    int left = 0, right = 0;
    
    // Scan from left to right
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(') {
            left++;
        } else {
            right++;
        }
        
        if (left == right) {
            maxLength = Math.max(maxLength, 2 * right);
        } else if (right > left) {
            left = right = 0;
        }
    }
    
    // Reset counters
    left = right = 0;
    
    // Scan from right to left
    for (int i = s.length() - 1; i >= 0; i--) {
        if (s.charAt(i) == ')') {
            right++;
        } else {
            left++;
        }
        
        if (left == right) {
            maxLength = Math.max(maxLength, 2 * left);
        } else if (left > right) {
            left = right = 0;
        }
    }
    
    return maxLength;
}
``` 