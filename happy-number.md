# Happy Number

**Difficulty**: Easy  
**Tags**: Hash Table, Math, Two Pointers

## Question
Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:
- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
- Those numbers for which this process ends in 1 are happy.

Return true if `n` is a happy number, and false if not.

## Example Input/Output
**Input**: n = 19  
**Output**: true  
**Explanation**:  
1² + 9² = 1 + 81 = 82  
8² + 2² = 64 + 4 = 68  
6² + 8² = 36 + 64 = 100  
1² + 0² + 0² = 1 + 0 + 0 = 1

**Input**: n = 2  
**Output**: false  
**Explanation**: The process does not end in 1, but in a cycle: 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4

## Solution Algorithm
There are two main approaches to solve this problem:

### Approach 1: Using a Hash Set to Detect Cycles
1. Create a hash set to keep track of numbers we've seen.
2. Calculate the sum of squares of digits.
3. If the sum is 1, return true.
4. If the sum is already in the hash set, we've detected a cycle, so return false.
5. Otherwise, add the sum to the hash set and repeat the process with the sum.

### Approach 2: Using Floyd's Cycle-Finding Algorithm (Tortoise and Hare)
1. Use two pointers, slow and fast, both starting at n.
2. The slow pointer calculates the sum of squares once per iteration.
3. The fast pointer calculates the sum of squares twice per iteration.
4. If they meet at 1, return true.
5. If they meet at any other number, we've detected a cycle, so return false.

## Solution Code
```java
// Approach 1: Using a Hash Set
public boolean isHappy(int n) {
    Set<Integer> seen = new HashSet<>();
    
    while (n != 1 && !seen.contains(n)) {
        seen.add(n);
        n = getNext(n);
    }
    
    return n == 1;
}

private int getNext(int n) {
    int sum = 0;
    
    while (n > 0) {
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }
    
    return sum;
}
```

```java
// Approach 2: Floyd's Cycle-Finding Algorithm
public boolean isHappy(int n) {
    int slow = n;
    int fast = getNext(n);
    
    while (fast != 1 && slow != fast) {
        slow = getNext(slow);
        fast = getNext(getNext(fast));
    }
    
    return fast == 1;
}

private int getNext(int n) {
    int sum = 0;
    
    while (n > 0) {
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }
    
    return sum;
}
``` 