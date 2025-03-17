# Reverse Bits

**Difficulty**: Easy  
**Tags**: Bit Manipulation, Divide and Conquer

## Question
Reverse bits of a given 32 bits unsigned integer.

Note:
- Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.

## Example Input/Output
**Input**: n = 00000010100101000001111010011100 (decimal: 43261596)  
**Output**: 00111001011110000010100101000000 (decimal: 964176192)  
**Explanation**: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 whose binary representation is 00111001011110000010100101000000.

**Input**: n = 11111111111111111111111111111101 (decimal: 4294967293)  
**Output**: 10111111111111111111111111111111 (decimal: 3221225471)  
**Explanation**: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 whose binary representation is 10111111111111111111111111111111.

## Solution Algorithm
There are several approaches to reverse the bits of an integer:

### Approach 1: Bit-by-Bit Reversal
1. Initialize the result to 0.
2. Iterate through all 32 bits of the input integer:
   - Extract the rightmost bit of the input.
   - Left shift the result by 1 and add the extracted bit.
   - Right shift the input by 1.
3. Return the result.

### Approach 2: Divide and Conquer
1. Swap adjacent bits (1-bit with 2-bit, 3-bit with 4-bit, etc.).
2. Swap adjacent pairs of bits (bits 1-2 with bits 3-4, bits 5-6 with bits 7-8, etc.).
3. Continue doubling the swap width until the entire 32-bit integer is reversed.

### Approach 3: Lookup Table
1. Create a lookup table for the reverse of each byte (0-255).
2. Split the 32-bit integer into 4 bytes.
3. Reverse each byte using the lookup table and combine them in reverse order.

## Solution Code
```java
// Approach 1: Bit-by-Bit Reversal
public int reverseBits(int n) {
    int result = 0;
    
    for (int i = 0; i < 32; i++) {
        // Left shift result by 1
        result <<= 1;
        
        // Add the rightmost bit of n to result
        result |= (n & 1);
        
        // Right shift n by 1
        n >>>= 1;
    }
    
    return result;
}
```

```java
// Approach 2: Divide and Conquer
public int reverseBits(int n) {
    // Swap adjacent bits
    n = ((n & 0x55555555) << 1) | ((n >>> 1) & 0x55555555);
    
    // Swap adjacent pairs of bits
    n = ((n & 0x33333333) << 2) | ((n >>> 2) & 0x33333333);
    
    // Swap adjacent nibbles (4 bits)
    n = ((n & 0x0F0F0F0F) << 4) | ((n >>> 4) & 0x0F0F0F0F);
    
    // Swap adjacent bytes
    n = ((n & 0x00FF00FF) << 8) | ((n >>> 8) & 0x00FF00FF);
    
    // Swap adjacent 16-bit chunks
    n = (n << 16) | (n >>> 16);
    
    return n;
}
```

```java
// Approach 3: Lookup Table
public int reverseBits(int n) {
    // Create a lookup table for the reverse of each byte
    int[] reverseByte = new int[256];
    for (int i = 0; i < 256; i++) {
        reverseByte[i] = (i & 0x01) << 7 | (i & 0x02) << 5 |
                         (i & 0x04) << 3 | (i & 0x08) << 1 |
                         (i & 0x10) >>> 1 | (i & 0x20) >>> 3 |
                         (i & 0x40) >>> 5 | (i & 0x80) >>> 7;
    }
    
    // Reverse each byte and combine them in reverse order
    return (reverseByte[n & 0xff] << 24) |
           (reverseByte[(n >>> 8) & 0xff] << 16) |
           (reverseByte[(n >>> 16) & 0xff] << 8) |
           (reverseByte[(n >>> 24) & 0xff]);
}
``` 