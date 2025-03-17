# Number of 1 Bits

**Difficulty**: Easy  
**Tags**: Bit Manipulation

## Question
Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

Note:
- Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 3, the input represents the signed integer `-3`.

## Example Input/Output
**Input**: n = 00000000000000000000000000001011  
**Output**: 3  
**Explanation**: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.

**Input**: n = 00000000000000000000000010000000  
**Output**: 1  
**Explanation**: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.

**Input**: n = 11111111111111111111111111111101  
**Output**: 31  
**Explanation**: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.

## Solution Algorithm
There are several approaches to count the number of 1 bits in an integer:

### Approach 1: Loop and Bit Shift
1. Initialize a counter to 0.
2. Check the least significant bit (LSB) of the number.
3. If the LSB is 1, increment the counter.
4. Right shift the number by 1 bit.
5. Repeat steps 2-4 until the number becomes 0.

### Approach 2: Brian Kernighan's Algorithm
1. Initialize a counter to 0.
2. While the number is not 0:
   - Use the operation `n & (n - 1)` to clear the least significant 1 bit.
   - Increment the counter.
3. Return the counter.

### Approach 3: Lookup Table
1. Create a lookup table for the number of 1 bits in each byte (0-255).
2. Split the 32-bit integer into 4 bytes.
3. Sum the number of 1 bits in each byte using the lookup table.

## Solution Code
```java
// Approach 1: Loop and Bit Shift
public int hammingWeight(int n) {
    int count = 0;
    
    // Need to use unsigned right shift (>>>) for Java
    // since Java uses signed integers
    for (int i = 0; i < 32; i++) {
        if ((n & 1) == 1) {
            count++;
        }
        n >>>= 1;
    }
    
    return count;
}
```

```java
// Approach 2: Brian Kernighan's Algorithm
public int hammingWeight(int n) {
    int count = 0;
    
    while (n != 0) {
        n &= (n - 1); // Clear the least significant 1 bit
        count++;
    }
    
    return count;
}
```

```java
// Approach 3: Lookup Table
public int hammingWeight(int n) {
    // Lookup table for number of 1 bits in each byte (0-255)
    int[] bitCounts = new int[256];
    for (int i = 0; i < 256; i++) {
        bitCounts[i] = (i & 1) + bitCounts[i / 2];
    }
    
    // Count bits in each byte of the 32-bit integer
    return bitCounts[n & 0xff] + 
           bitCounts[(n >>> 8) & 0xff] + 
           bitCounts[(n >>> 16) & 0xff] + 
           bitCounts[(n >>> 24) & 0xff];
}
``` 