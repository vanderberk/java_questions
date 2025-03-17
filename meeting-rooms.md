# Meeting Rooms

**Difficulty**: Easy  
**Tags**: Array, Sorting

## Question
Given an array of meeting time intervals where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.

## Example Input/Output
**Input**: intervals = [[0,30],[5,10],[15,20]]  
**Output**: false  
**Explanation**: The person cannot attend all meetings because the meeting at time 5 overlaps with the meeting at time 0.

**Input**: intervals = [[7,10],[2,4]]  
**Output**: true  
**Explanation**: The person can attend all meetings because the meetings do not overlap.

## Solution Algorithm
To determine if a person can attend all meetings, we need to check if any two meetings overlap. We can solve this problem using the following approach:

1. Sort the intervals based on the start time.
2. Iterate through the sorted intervals and check if the current meeting's start time is less than the previous meeting's end time.
3. If there's an overlap, return false.
4. If no overlaps are found, return true.

## Solution Code
```java
public boolean canAttendMeetings(int[][] intervals) {
    // Sort intervals based on start time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    // Check for overlaps
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] < intervals[i - 1][1]) {
            return false; // Overlap found
        }
    }
    
    return true; // No overlaps
}
```

```java
// Alternative implementation with a more explicit sorting comparator
public boolean canAttendMeetings(int[][] intervals) {
    if (intervals == null || intervals.length <= 1) {
        return true;
    }
    
    // Sort intervals based on start time
    Arrays.sort(intervals, new Comparator<int[]>() {
        public int compare(int[] a, int[] b) {
            return a[0] - b[0];
        }
    });
    
    // Check for overlaps
    for (int i = 0; i < intervals.length - 1; i++) {
        if (intervals[i][1] > intervals[i + 1][0]) {
            return false; // Overlap found
        }
    }
    
    return true; // No overlaps
}
```

```java
// Implementation using Interval class (if available)
public boolean canAttendMeetings(Interval[] intervals) {
    if (intervals == null || intervals.length <= 1) {
        return true;
    }
    
    // Sort intervals based on start time
    Arrays.sort(intervals, (a, b) -> a.start - b.start);
    
    // Check for overlaps
    for (int i = 0; i < intervals.length - 1; i++) {
        if (intervals[i].end > intervals[i + 1].start) {
            return false; // Overlap found
        }
    }
    
    return true; // No overlaps
}

// Definition for an interval
class Interval {
    int start;
    int end;
    Interval() { start = 0; end = 0; }
    Interval(int s, int e) { start = s; end = e; }
}
``` 