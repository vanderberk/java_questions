# Count Primes

**Difficulty**: Easy  
**Tags**: Math, Number Theory, Sieve of Eratosthenes

## Question
Given an integer `n`, return the number of prime numbers that are strictly less than `n`.

## Example Input/Output
**Input**: n = 10  
**Output**: 4  
**Explanation**: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

**Input**: n = 0  
**Output**: 0

**Input**: n = 1  
**Output**: 0

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Brute Force
1. For each number from 2 to n-1, check if it's prime.
2. Count the number of prime numbers found.

### Approach 2: Sieve of Eratosthenes
1. Create a boolean array `isPrime` of size n, initialized to true.
2. Set `isPrime[0]` and `isPrime[1]` to false (0 and 1 are not prime).
3. For each number i from 2 to sqrt(n):
   - If `isPrime[i]` is true, mark all multiples of i as not prime.
4. Count the number of true values in the `isPrime` array.

The Sieve of Eratosthenes is much more efficient than the brute force approach, especially for large values of n.

## Solution Code
```java
// Approach 1: Brute Force
public int countPrimes(int n) {
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (isPrime(i)) {
            count++;
        }
    }
    return count;
}

private boolean isPrime(int num) {
    if (num <= 1) {
        return false;
    }
    // Check from 2 to sqrt(num)
    for (int i = 2; i * i <= num; i++) {
        if (num % i == 0) {
            return false;
        }
    }
    return true;
}
```

```java
// Approach 2: Sieve of Eratosthenes
public int countPrimes(int n) {
    if (n <= 2) {
        return 0;
    }
    
    // Initialize isPrime array
    boolean[] isPrime = new boolean[n];
    for (int i = 2; i < n; i++) {
        isPrime[i] = true;
    }
    
    // Apply Sieve of Eratosthenes
    for (int i = 2; i * i < n; i++) {
        if (!isPrime[i]) {
            continue;
        }
        
        // Mark all multiples of i as not prime
        for (int j = i * i; j < n; j += i) {
            isPrime[j] = false;
        }
    }
    
    // Count prime numbers
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (isPrime[i]) {
            count++;
        }
    }
    
    return count;
}
```

```java
// Approach 2 (Optimized): Sieve of Eratosthenes
public int countPrimes(int n) {
    if (n <= 2) {
        return 0;
    }
    
    // Use a boolean array to mark non-prime numbers
    boolean[] notPrime = new boolean[n];
    int count = 0;
    
    for (int i = 2; i < n; i++) {
        if (!notPrime[i]) {
            count++;
            
            // Mark all multiples of i as not prime
            // Start from i*i because all smaller multiples have already been marked
            for (long j = (long) i * i; j < n; j += i) {
                notPrime[(int) j] = true;
            }
        }
    }
    
    return count;
}
``` 