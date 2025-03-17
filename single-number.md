# Single Number

**Difficulty**: Easy  
**Tags**: Array, Bit Manipulation, Hash Table

## Question
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

## Example Input/Output
**Input**: nums = [2,2,1]  
**Output**: 1

**Input**: nums = [4,1,2,1,2]  
**Output**: 4

**Input**: nums = [1]  
**Output**: 1

## Solution Algorithm
1. Use the XOR bitwise operation to find the single number
2. XOR has the property that a number XORed with itself equals 0, and a number XORed with 0 equals itself
3. By XORing all numbers in the array, the numbers that appear twice will cancel out, leaving only the single number

## Solution Code
```java
public int singleNumber(int[] nums) {
    int result = 0;
    
    for (int num : nums) {
        result ^= num;
    }
    
    return result;
}
``` 