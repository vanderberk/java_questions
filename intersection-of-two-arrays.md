# Intersection of Two Arrays

**Difficulty**: Easy  
**Tags**: Array, Hash Table, Two Pointers, Binary Search, Sorting

## Question
Given two integer arrays `nums1` and `nums2`, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

## Example Input/Output
**Input**: nums1 = [1,2,2,1], nums2 = [2,2]  
**Output**: [2]

**Input**: nums1 = [4,9,5], nums2 = [9,4,9,8,4]  
**Output**: [9,4]  
**Explanation**: [4,9] is also accepted.

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Using a Hash Set
1. Create a set from the first array to remove duplicates
2. Create a result set to store the intersection
3. Iterate through the second array and add elements to the result set if they exist in the first set
4. Convert the result set to an array and return it

### Approach 2: Using Two Pointers (if arrays are sorted)
1. Sort both arrays
2. Use two pointers to iterate through the arrays
3. If the elements are equal, add to the result (if not already added)
4. If the element in the first array is smaller, move the first pointer
5. If the element in the second array is smaller, move the second pointer

### Approach 3: Using Binary Search (if one array is much larger)
1. Sort the smaller array
2. For each element in the larger array, use binary search to check if it exists in the smaller array
3. Add to the result if found (and not already added)

## Solution Code
```java
// Approach 1: Using Hash Set
public int[] intersection(int[] nums1, int[] nums2) {
    // Create a set from the first array to remove duplicates
    Set<Integer> set1 = new HashSet<>();
    for (int num : nums1) {
        set1.add(num);
    }
    
    // Create a set to store the intersection
    Set<Integer> resultSet = new HashSet<>();
    
    // Check each element in nums2
    for (int num : nums2) {
        if (set1.contains(num)) {
            resultSet.add(num);
        }
    }
    
    // Convert set to array
    int[] result = new int[resultSet.size()];
    int i = 0;
    for (int num : resultSet) {
        result[i++] = num;
    }
    
    return result;
}
```

```java
// Approach 2: Using Two Pointers (if arrays are sorted)
public int[] intersection(int[] nums1, int[] nums2) {
    // Sort both arrays
    Arrays.sort(nums1);
    Arrays.sort(nums2);
    
    // Use a set to store unique intersection elements
    Set<Integer> resultSet = new HashSet<>();
    
    // Two pointers
    int i = 0, j = 0;
    
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] < nums2[j]) {
            i++;
        } else if (nums1[i] > nums2[j]) {
            j++;
        } else {
            // Found an intersection
            resultSet.add(nums1[i]);
            i++;
            j++;
        }
    }
    
    // Convert set to array
    int[] result = new int[resultSet.size()];
    int index = 0;
    for (int num : resultSet) {
        result[index++] = num;
    }
    
    return result;
}
```

```java
// Approach 3: Using Binary Search (if one array is much larger)
public int[] intersection(int[] nums1, int[] nums2) {
    // Ensure nums1 is the smaller array for efficiency
    if (nums1.length > nums2.length) {
        return intersection(nums2, nums1);
    }
    
    // Sort the smaller array for binary search
    Arrays.sort(nums1);
    
    // Use a set to store unique intersection elements
    Set<Integer> resultSet = new HashSet<>();
    
    // For each element in nums2, check if it exists in nums1
    for (int num : nums2) {
        if (binarySearch(nums1, num) && !resultSet.contains(num)) {
            resultSet.add(num);
        }
    }
    
    // Convert set to array
    int[] result = new int[resultSet.size()];
    int i = 0;
    for (int num : resultSet) {
        result[i++] = num;
    }
    
    return result;
}

private boolean binarySearch(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return true;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return false;
}
``` 