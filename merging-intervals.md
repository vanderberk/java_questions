# Merging Intervals

**Difficulty**: Medium  
**Tags**: Arrays, Sorting, Intervals

## Question
Given an array of intervals where intervals[i] = [start_i, end_i], merge all overlapping intervals and return an array of non-overlapping intervals that cover all the intervals in the input.

## Example Input/Output
**Input**: [[1,3],[2,6],[8,10],[15,18]]  
**Output**: [[1,6],[8,10],[15,18]]

**Input**: [[1,4],[4,5]]  
**Output**: [[1,5]]

## Solution Algorithm
1. Sort the intervals by their start time
2. Initialize the result list with the first interval
3. Iterate through the remaining intervals:
   - If the current interval overlaps with the last interval in the result, merge them
   - Otherwise, add the current interval to the result
4. Return the result list

## Solution Code
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) {
        return intervals;
    }
    
    // Sort by start time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    List<int[]> result = new ArrayList<>();
    int[] currentInterval = intervals[0];
    result.add(currentInterval);
    
    for (int i = 1; i < intervals.length; i++) {
        int currentEnd = currentInterval[1];
        int nextStart = intervals[i][0];
        int nextEnd = intervals[i][1];
        
        // If overlapping
        if (currentEnd >= nextStart) {
            // Merge
            currentInterval[1] = Math.max(currentEnd, nextEnd);
        } else {
            // Not overlapping, add to result
            currentInterval = intervals[i];
            result.add(currentInterval);
        }
    }
    
    return result.toArray(new int[result.size()][]);
} 