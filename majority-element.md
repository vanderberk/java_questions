# Majority Element

**Difficulty**: Easy  
**Tags**: Array, Hash Table, Divide and Conquer, Sorting, Counting

## Question
Given an array `nums` of size n, return the majority element.

The majority element is the element that appears more than ⌊n/2⌋ times. You may assume that the majority element always exists in the array.

## Example Input/Output
**Input**: nums = [3,2,3]  
**Output**: 3

**Input**: nums = [2,2,1,1,1,2,2]  
**Output**: 2

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Brute Force
1. For each element in the array, count its occurrences.
2. Return the element that appears more than ⌊n/2⌋ times.

### Approach 2: Hash Map
1. Use a hash map to count the occurrences of each element.
2. Return the element with the highest count.

### Approach 3: Sorting
1. Sort the array.
2. Return the element at index ⌊n/2⌋, which will be the majority element.

### Approach 4: Boyer-Moore Voting Algorithm
1. Initialize a candidate element and a counter to 0.
2. Iterate through the array:
   - If the counter is 0, set the current element as the candidate.
   - If the current element is the same as the candidate, increment the counter.
   - If the current element is different from the candidate, decrement the counter.
3. Return the candidate, which will be the majority element.

## Solution Code
```java
// Approach 1: Brute Force
public int majorityElement(int[] nums) {
    int majorityCount = nums.length / 2;
    
    for (int num : nums) {
        int count = 0;
        for (int elem : nums) {
            if (elem == num) {
                count++;
            }
        }
        
        if (count > majorityCount) {
            return num;
        }
    }
    
    return -1; // No majority element found (though the problem guarantees one exists)
}
```

```java
// Approach 2: Hash Map
public int majorityElement(int[] nums) {
    Map<Integer, Integer> counts = new HashMap<>();
    
    for (int num : nums) {
        counts.put(num, counts.getOrDefault(num, 0) + 1);
    }
    
    Map.Entry<Integer, Integer> majorityEntry = null;
    for (Map.Entry<Integer, Integer> entry : counts.entrySet()) {
        if (majorityEntry == null || entry.getValue() > majorityEntry.getValue()) {
            majorityEntry = entry;
        }
    }
    
    return majorityEntry.getKey();
}
```

```java
// Approach 3: Sorting
public int majorityElement(int[] nums) {
    Arrays.sort(nums);
    return nums[nums.length / 2];
}
```

```java
// Approach 4: Boyer-Moore Voting Algorithm
public int majorityElement(int[] nums) {
    int count = 0;
    Integer candidate = null;
    
    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        
        count += (num == candidate) ? 1 : -1;
    }
    
    return candidate;
}
``` 