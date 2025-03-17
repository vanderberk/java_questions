# Sliding Window Maximum

**Difficulty**: Hard  
**Tags**: Array, Sliding Window, Deque, Heap, Monotonic Queue

## Question
You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

## Example Input/Output
**Input**: nums = [1,3,-1,-3,5,3,6,7], k = 3  
**Output**: [3,3,5,5,6,7]  
**Explanation**:  
```
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Input**: nums = [1], k = 1  
**Output**: [1]

## Solution Algorithm
We can solve this problem efficiently using a deque (double-ended queue) to maintain a monotonic decreasing queue of indices:

1. Create a deque to store indices of elements in the current window
2. Create a result array to store the maximum values
3. Process the first k elements:
   - For each element, remove all elements from the back of the deque that are smaller than the current element
   - Add the current index to the back of the deque
4. The front of the deque now contains the index of the maximum element in the first window
5. For the remaining elements:
   - Add the maximum element of the current window to the result
   - Remove elements from the front of the deque that are outside the current window
   - Remove all elements from the back of the deque that are smaller than the current element
   - Add the current index to the back of the deque
6. Add the maximum element of the last window to the result
7. Return the result array

## Solution Code
```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k <= 0) {
        return new int[0];
    }
    
    int n = nums.length;
    int[] result = new int[n - k + 1];
    int resultIdx = 0;
    
    // Deque to store indices of elements in current window
    // Elements are stored in decreasing order (largest at front)
    Deque<Integer> deque = new ArrayDeque<>();
    
    for (int i = 0; i < nums.length; i++) {
        // Remove elements outside the current window
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }
        
        // Remove all elements smaller than the current element from the back
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        
        // Add current element's index
        deque.offerLast(i);
        
        // If we've processed at least k elements, add the maximum to result
        if (i >= k - 1) {
            result[resultIdx++] = nums[deque.peekFirst()];
        }
    }
    
    return result;
}
```

// Alternative approach using a max heap (less efficient)
```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k <= 0) {
        return new int[0];
    }
    
    int n = nums.length;
    int[] result = new int[n - k + 1];
    
    // Max heap that stores pairs of (value, index)
    PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> b[0] - a[0]);
    
    for (int i = 0; i < nums.length; i++) {
        // Add current element to the heap
        maxHeap.offer(new int[]{nums[i], i});
        
        // If we've processed at least k elements
        if (i >= k - 1) {
            // Remove elements that are outside the current window
            while (!maxHeap.isEmpty() && maxHeap.peek()[1] < i - k + 1) {
                maxHeap.poll();
            }
            
            // The top of the heap is the maximum in the current window
            result[i - k + 1] = maxHeap.peek()[0];
        }
    }
    
    return result;
}
``` 