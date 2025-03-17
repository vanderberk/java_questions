# Add Binary

**Difficulty**: Easy  
**Tags**: Math, String, Bit Manipulation

## Question
Given two binary strings `a` and `b`, return their sum as a binary string.

## Example Input/Output
**Input**: a = "11", b = "1"  
**Output**: "100"

**Input**: a = "1010", b = "1011"  
**Output**: "10101"

## Solution Algorithm
We can solve this problem using several approaches:

### Approach 1: Bit-by-Bit Addition
1. Start from the least significant bit (rightmost) of both strings.
2. Perform binary addition with carry.
3. Build the result string from right to left.

### Approach 2: Built-in Functions
1. Convert both binary strings to integers.
2. Add the integers.
3. Convert the sum back to a binary string.

### Approach 3: Bit Manipulation
1. Convert both binary strings to integers.
2. Use bitwise operations to perform the addition.
3. Convert the result back to a binary string.

## Solution Code
```java
// Approach 1: Bit-by-Bit Addition
public String addBinary(String a, String b) {
    StringBuilder result = new StringBuilder();
    int i = a.length() - 1;
    int j = b.length() - 1;
    int carry = 0;
    
    while (i >= 0 || j >= 0) {
        int sum = carry;
        
        if (i >= 0) {
            sum += a.charAt(i--) - '0';
        }
        
        if (j >= 0) {
            sum += b.charAt(j--) - '0';
        }
        
        result.append(sum % 2);
        carry = sum / 2;
    }
    
    if (carry > 0) {
        result.append(carry);
    }
    
    return result.reverse().toString();
}
```

```java
// Approach 2: Built-in Functions
// Note: This approach has limitations for very large binary strings
public String addBinary(String a, String b) {
    try {
        return Integer.toBinaryString(Integer.parseInt(a, 2) + Integer.parseInt(b, 2));
    } catch (NumberFormatException e) {
        // For large binary strings, use BigInteger
        java.math.BigInteger bigA = new java.math.BigInteger(a, 2);
        java.math.BigInteger bigB = new java.math.BigInteger(b, 2);
        java.math.BigInteger sum = bigA.add(bigB);
        return sum.toString(2);
    }
}
```

```java
// Approach 3: Bit Manipulation
// Note: This approach has limitations for very large binary strings
public String addBinary(String a, String b) {
    int aInt = Integer.parseInt(a, 2);
    int bInt = Integer.parseInt(b, 2);
    
    while (bInt != 0) {
        // XOR gives sum without carry
        int sum = aInt ^ bInt;
        
        // AND with left shift gives carry
        int carry = (aInt & bInt) << 1;
        
        aInt = sum;
        bInt = carry;
    }
    
    return Integer.toBinaryString(aInt);
}
``` 