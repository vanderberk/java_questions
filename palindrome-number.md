# Palindrome Number

**Difficulty**: Easy  
**Tags**: Math

## Question
Given an integer `x`, return `true` if `x` is a palindrome, and `false` otherwise.

A palindrome is a number that reads the same backward as forward. For example, 121 is a palindrome while 123 is not.

## Example Input/Output
**Input**: x = 121  
**Output**: true  
**Explanation**: 121 reads as 121 from left to right and from right to left.

**Input**: x = -121  
**Output**: false  
**Explanation**: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

**Input**: x = 10  
**Output**: false  
**Explanation**: Reads 01 from right to left. Therefore it is not a palindrome.

## Solution Algorithm
There are two main approaches to solve this problem:

### Approach 1: Convert to String
1. Convert the integer to a string
2. Check if the string is equal to its reverse

### Approach 2: Reverse the Number
1. If the number is negative, return false (negative numbers can't be palindromes)
2. If the number is positive and ends with 0, it can only be a palindrome if it's 0
3. Reverse the digits of the number
4. Compare the reversed number with the original number

## Solution Code
```java
// Approach 1: Convert to String
public boolean isPalindrome(int x) {
    // Convert to string
    String str = String.valueOf(x);
    
    // Check if the string is equal to its reverse
    int left = 0;
    int right = str.length() - 1;
    
    while (left < right) {
        if (str.charAt(left) != str.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}
```

```java
// Approach 2: Reverse the Number
public boolean isPalindrome(int x) {
    // Negative numbers are not palindromes
    if (x < 0) {
        return false;
    }
    
    // If the last digit is 0, the first digit must also be 0
    // The only such number is 0 itself
    if (x > 0 && x % 10 == 0) {
        return false;
    }
    
    int reversed = 0;
    int original = x;
    
    // Reverse the number
    while (x > 0) {
        int digit = x % 10;
        reversed = reversed * 10 + digit;
        x /= 10;
    }
    
    return original == reversed;
}
```

```java
// Approach 3: Reverse Half of the Number (more efficient)
public boolean isPalindrome(int x) {
    // Negative numbers are not palindromes
    // If the last digit is 0, the first digit must also be 0 (only 0 is valid)
    if (x < 0 || (x > 0 && x % 10 == 0)) {
        return false;
    }
    
    int reversed = 0;
    
    // Reverse only half of the number
    // When reversed >= x, we've processed half or more than half of the digits
    while (x > reversed) {
        reversed = reversed * 10 + x % 10;
        x /= 10;
    }
    
    // For even number of digits: x == reversed
    // For odd number of digits: x == reversed / 10 (to ignore the middle digit)
    return x == reversed || x == reversed / 10;
}
``` 