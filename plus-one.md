# Plus One

**Difficulty**: Easy  
**Tags**: Array, Math

## Question
You are given a large integer represented as an integer array `digits`, where each `digits[i]` is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

## Example Input/Output
**Input**: digits = [1,2,3]  
**Output**: [1,2,4]  
**Explanation**: The array represents the integer 123. Incrementing by one gives 123 + 1 = 124. Thus, the result should be [1,2,4].

**Input**: digits = [4,3,2,1]  
**Output**: [4,3,2,2]  
**Explanation**: The array represents the integer 4321. Incrementing by one gives 4321 + 1 = 4322. Thus, the result should be [4,3,2,2].

**Input**: digits = [9]  
**Output**: [1,0]  
**Explanation**: The array represents the integer 9. Incrementing by one gives 9 + 1 = 10. Thus, the result should be [1,0].

## Solution Algorithm
1. Start from the least significant digit (rightmost)
2. Add 1 to it
3. If the result is less than 10, update the digit and return the array
4. If the result is 10, set the current digit to 0 and carry the 1 to the next digit
5. Repeat steps 3-4 for each digit moving from right to left
6. If we reach the most significant digit and still have a carry, create a new array with an additional digit at the beginning

## Solution Code
```java
public int[] plusOne(int[] digits) {
    int n = digits.length;
    
    // Start from the least significant digit
    for (int i = n - 1; i >= 0; i--) {
        // Add 1 to the current digit
        digits[i]++;
        
        // If the result is less than 10, we're done
        if (digits[i] < 10) {
            return digits;
        }
        
        // If the result is 10, set it to 0 and carry the 1
        digits[i] = 0;
    }
    
    // If we get here, it means we had all 9's
    // Create a new array with an additional digit
    int[] result = new int[n + 1];
    result[0] = 1; // The first digit is 1, rest are 0's by default
    
    return result;
}
```

// Alternative implementation with clearer logic
```java
public int[] plusOne(int[] digits) {
    for (int i = digits.length - 1; i >= 0; i--) {
        // If the current digit is less than 9, we can simply increment it and return
        if (digits[i] < 9) {
            digits[i]++;
            return digits;
        }
        
        // If the current digit is 9, set it to 0 and continue to the next digit
        digits[i] = 0;
    }
    
    // If we're here, it means all digits were 9
    // Create a new array with one more digit
    int[] newDigits = new int[digits.length + 1];
    newDigits[0] = 1; // First digit is 1, rest are 0's by default
    
    return newDigits;
}
``` 