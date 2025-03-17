# Longest Consecutive Sequence

**Difficulty**: Hard  
**Tags**: Array, Hash Table, Union Find

## Question
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

## Example Input/Output
**Input**: nums = [100,4,200,1,3,2]  
**Output**: 4  
**Explanation**: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

**Input**: nums = [0,3,7,2,5,8,4,6,0,1]  
**Output**: 9

## Solution Algorithm
We can solve this problem in O(n) time using a hash set:

1. Add all numbers from the input array to a hash set for O(1) lookups
2. Iterate through each number in the array:
   - For each number, check if it's the start of a sequence (i.e., num-1 is not in the set)
   - If it's the start of a sequence, count the length of the consecutive sequence starting from this number
   - Update the maximum length if the current sequence is longer
3. Return the maximum length found

## Solution Code
```java
public int longestConsecutive(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    // Add all numbers to a hash set for O(1) lookups
    Set<Integer> numSet = new HashSet<>();
    for (int num : nums) {
        numSet.add(num);
    }
    
    int maxLength = 0;
    
    // Check each number to see if it's the start of a sequence
    for (int num : nums) {
        // Only check for sequences starting with the smallest number in the sequence
        // This ensures O(n) time complexity
        if (!numSet.contains(num - 1)) {
            int currentNum = num;
            int currentLength = 1;
            
            // Count the length of the consecutive sequence
            while (numSet.contains(currentNum + 1)) {
                currentNum++;
                currentLength++;
            }
            
            // Update the maximum length
            maxLength = Math.max(maxLength, currentLength);
        }
    }
    
    return maxLength;
}
```

// Alternative implementation using Union-Find
```java
public int longestConsecutive(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    // Map to store the parent of each number
    Map<Integer, Integer> parent = new HashMap<>();
    // Map to store the size of each set
    Map<Integer, Integer> size = new HashMap<>();
    // Set to remove duplicates
    Set<Integer> numSet = new HashSet<>();
    
    // Initialize parent and size for each number
    for (int num : nums) {
        numSet.add(num);
        parent.put(num, num);
        size.put(num, 1);
    }
    
    // Union consecutive numbers
    for (int num : numSet) {
        if (numSet.contains(num + 1)) {
            union(parent, size, num, num + 1);
        }
    }
    
    // Find the maximum size
    int maxSize = 0;
    for (int s : size.values()) {
        maxSize = Math.max(maxSize, s);
    }
    
    return maxSize;
}

private int find(Map<Integer, Integer> parent, int x) {
    if (parent.get(x) != x) {
        parent.put(x, find(parent, parent.get(x)));
    }
    return parent.get(x);
}

private void union(Map<Integer, Integer> parent, Map<Integer, Integer> size, int x, int y) {
    int rootX = find(parent, x);
    int rootY = find(parent, y);
    
    if (rootX != rootY) {
        parent.put(rootX, rootY);
        size.put(rootY, size.get(rootX) + size.get(rootY));
    }
}
``` 