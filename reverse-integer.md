# Reverse Integer

**Difficulty**: Easy  
**Tags**: Math, Integer Manipulation

## Question
Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-2^31, 2^31 - 1], then return 0.

## Example Input/Output
**Input**: x = 123  
**Output**: 321

**Input**: x = -123  
**Output**: -321

**Input**: x = 120  
**Output**: 21

## Solution Algorithm
1. Initialize a result variable to 0
2. While the input is not 0:
   - Extract the last digit of the input using the modulo operator (%)
   - Check for potential overflow before adding the digit to the result
   - Add the digit to the result (after multiplying the current result by 10)
   - Remove the last digit from the input by integer division (/)
3. Return the result

## Solution Code
```java
public int reverse(int x) {
    int result = 0;
    
    while (x != 0) {
        int lastDigit = x % 10;
        
        // Check for potential overflow
        if (result > Integer.MAX_VALUE / 10 || (result == Integer.MAX_VALUE / 10 && lastDigit > 7)) {
            return 0;
        }
        if (result < Integer.MIN_VALUE / 10 || (result == Integer.MIN_VALUE / 10 && lastDigit < -8)) {
            return 0;
        }
        
        result = result * 10 + lastDigit;
        x /= 10;
    }
    
    return result;
} 