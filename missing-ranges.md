# Missing Ranges

**Difficulty**: Easy  
**Tags**: Array

## Question
You are given an inclusive range [lower, upper] and a sorted integer array nums, where all elements are in the inclusive range.

Return the smallest sorted list of ranges that cover every missing number in the range. That is, no element in the range [lower, upper] is outside of any range in your list.

Each range [a,b] in the list should be output as:
- "a->b" if a != b
- "a" if a == b

## Example Input/Output
**Input**: nums = [0,1,3,50,75], lower = 0, upper = 99  
**Output**: ["2","4->49","51->74","76->99"]  
**Explanation**: The missing ranges are: [2,2], [4,49], [51,74], [76,99]

**Input**: nums = [], lower = 1, upper = 1  
**Output**: ["1"]  
**Explanation**: The entire range [1,1] is missing.

**Input**: nums = [], lower = -3, upper = -1  
**Output**: ["-3->-1"]  
**Explanation**: The entire range [-3,-1] is missing.

**Input**: nums = [-1], lower = -1, upper = -1  
**Output**: []  
**Explanation**: There are no missing ranges since there are no missing numbers.

## Solution Algorithm
We can solve this problem by iterating through the array and checking for gaps between consecutive elements:

1. Initialize an empty list to store the missing ranges.
2. Handle the case where the array is empty separately.
3. Check if there's a gap between the lower bound and the first element of the array.
4. Iterate through the array and check for gaps between consecutive elements.
5. Check if there's a gap between the last element of the array and the upper bound.
6. Format each missing range according to the requirements and add it to the result list.
7. Return the result list.

## Solution Code
```java
public List<String> findMissingRanges(int[] nums, int lower, int upper) {
    List<String> result = new ArrayList<>();
    
    // Handle empty array case
    if (nums.length == 0) {
        addRange(result, lower, upper);
        return result;
    }
    
    // Check if there's a gap between lower bound and first element
    if (lower < nums[0]) {
        addRange(result, lower, nums[0] - 1);
    }
    
    // Check for gaps between consecutive elements
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] > nums[i - 1] + 1) {
            addRange(result, nums[i - 1] + 1, nums[i] - 1);
        }
    }
    
    // Check if there's a gap between last element and upper bound
    if (upper > nums[nums.length - 1]) {
        addRange(result, nums[nums.length - 1] + 1, upper);
    }
    
    return result;
}

private void addRange(List<String> result, int start, int end) {
    if (start == end) {
        result.add(String.valueOf(start));
    } else {
        result.add(start + "->" + end);
    }
}
```

```java
// Alternative implementation with better handling of edge cases
public List<String> findMissingRanges(int[] nums, int lower, int upper) {
    List<String> result = new ArrayList<>();
    
    // Use long to handle potential integer overflow
    long prev = (long)lower - 1;
    
    for (int i = 0; i <= nums.length; i++) {
        long curr = (i == nums.length) ? (long)upper + 1 : nums[i];
        
        if (curr - prev > 1) {
            result.add(formatRange(prev + 1, curr - 1));
        }
        
        prev = curr;
    }
    
    return result;
}

private String formatRange(long start, long end) {
    return (start == end) ? String.valueOf(start) : start + "->" + end;
}
``` 