# Power of Three

**Difficulty**: Easy  
**Tags**: Math, Recursion

## Question
Given an integer `n`, return `true` if it is a power of three. Otherwise, return `false`.

An integer `n` is a power of three, if there exists an integer `x` such that `n == 3^x`.

## Example Input/Output
**Input**: n = 27  
**Output**: true  
**Explanation**: 27 = 3^3

**Input**: n = 0  
**Output**: false  
**Explanation**: 0 is not a power of 3

**Input**: n = 9  
**Output**: true  
**Explanation**: 9 = 3^2

**Input**: n = -1  
**Output**: false  
**Explanation**: -1 is not a power of 3

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Iterative Solution
1. If n is less than or equal to 0, return false
2. While n is divisible by 3, divide n by 3
3. Check if n is equal to 1 (which means it was a power of 3)

### Approach 2: Recursive Solution
1. If n is less than or equal to 0, return false
2. If n is equal to 1, return true (base case)
3. If n is not divisible by 3, return false
4. Recursively check if n/3 is a power of 3

### Approach 3: Mathematics Solution
1. The maximum power of 3 that fits in a 32-bit integer is 3^19 = 1162261467
2. If n is a power of 3, then 3^19 % n must be 0
3. This only works if n is positive

## Solution Code
```java
// Approach 1: Iterative Solution
public boolean isPowerOfThree(int n) {
    if (n <= 0) {
        return false;
    }
    
    while (n % 3 == 0) {
        n /= 3;
    }
    
    return n == 1;
}
```

```java
// Approach 2: Recursive Solution
public boolean isPowerOfThree(int n) {
    if (n <= 0) {
        return false;
    }
    
    if (n == 1) {
        return true;
    }
    
    if (n % 3 != 0) {
        return false;
    }
    
    return isPowerOfThree(n / 3);
}
```

```java
// Approach 3: Mathematics Solution
public boolean isPowerOfThree(int n) {
    if (n <= 0) {
        return false;
    }
    
    // 3^19 = 1162261467 is the largest power of 3 that fits in a 32-bit integer
    return 1162261467 % n == 0;
}
```

```java
// Approach 4: Using logarithms
public boolean isPowerOfThree(int n) {
    if (n <= 0) {
        return false;
    }
    
    // Calculate log base 3 of n
    double logBase3 = Math.log10(n) / Math.log10(3);
    
    // Check if logBase3 is an integer
    return Math.abs(logBase3 - Math.round(logBase3)) < 1e-10;
}
``` 