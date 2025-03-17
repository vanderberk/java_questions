# Contains Duplicate

**Difficulty**: Easy  
**Tags**: Array, Hash Table, Sorting

## Question
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

## Example Input/Output
**Input**: nums = [1,2,3,1]  
**Output**: true

**Input**: nums = [1,2,3,4]  
**Output**: false

**Input**: nums = [1,1,1,3,3,4,3,2,4,2]  
**Output**: true

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Brute Force
1. For each element in the array, check if it appears elsewhere in the array.
2. If any element appears twice, return true.
3. Otherwise, return false.

### Approach 2: Sorting
1. Sort the array.
2. Check if any adjacent elements are equal.
3. If any adjacent elements are equal, return true.
4. Otherwise, return false.

### Approach 3: Hash Set
1. Create a hash set to store elements we've seen.
2. Iterate through the array:
   - If the current element is already in the hash set, return true.
   - Otherwise, add the current element to the hash set.
3. If we finish iterating without finding any duplicates, return false.

## Solution Code
```java
// Approach 1: Brute Force
public boolean containsDuplicate(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] == nums[j]) {
                return true;
            }
        }
    }
    return false;
}
```

```java
// Approach 2: Sorting
public boolean containsDuplicate(int[] nums) {
    Arrays.sort(nums);
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1]) {
            return true;
        }
    }
    return false;
}
```

```java
// Approach 3: Hash Set
public boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (seen.contains(num)) {
            return true;
        }
        seen.add(num);
    }
    return false;
}
``` 