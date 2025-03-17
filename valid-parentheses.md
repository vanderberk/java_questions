# Valid Parentheses

**Difficulty**: Easy  
**Tags**: String, Stack

## Question
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

## Example Input/Output
**Input**: s = "()"  
**Output**: true

**Input**: s = "()[]{}"  
**Output**: true

**Input**: s = "(]"  
**Output**: false

**Input**: s = "([)]"  
**Output**: false

**Input**: s = "{[]}"  
**Output**: true

## Solution Algorithm
We can solve this problem using a stack data structure:

1. Initialize an empty stack.
2. Iterate through each character in the string:
   - If the character is an opening bracket (`(`, `{`, or `[`), push it onto the stack.
   - If the character is a closing bracket (`)`, `}`, or `]`):
     - If the stack is empty, return false (no matching opening bracket).
     - Pop the top element from the stack.
     - If the popped element is not the matching opening bracket for the current closing bracket, return false.
3. After processing all characters, if the stack is empty, return true (all brackets were matched).
   Otherwise, return false (some opening brackets were not closed).

## Solution Code
```java
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '{' || c == '[') {
            // Push opening brackets onto the stack
            stack.push(c);
        } else {
            // For closing brackets, check if the stack is empty
            if (stack.isEmpty()) {
                return false;
            }
            
            // Pop the top element and check if it matches the current closing bracket
            char top = stack.pop();
            if ((c == ')' && top != '(') || 
                (c == '}' && top != '{') || 
                (c == ']' && top != '[')) {
                return false;
            }
        }
    }
    
    // If the stack is empty, all brackets were matched
    return stack.isEmpty();
}
```

```java
// Alternative implementation using a HashMap to store bracket pairs
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    Map<Character, Character> bracketPairs = new HashMap<>();
    bracketPairs.put(')', '(');
    bracketPairs.put('}', '{');
    bracketPairs.put(']', '[');
    
    for (char c : s.toCharArray()) {
        // Check if the character is a closing bracket
        if (bracketPairs.containsKey(c)) {
            // If the stack is empty or the top element doesn't match, return false
            if (stack.isEmpty() || stack.pop() != bracketPairs.get(c)) {
                return false;
            }
        } else {
            // Push opening brackets onto the stack
            stack.push(c);
        }
    }
    
    // If the stack is empty, all brackets were matched
    return stack.isEmpty();
}
``` 