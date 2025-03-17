# Kth Largest Element in an Array

**Difficulty**: Medium  
**Tags**: Array, Divide and Conquer, Sorting, Heap, QuickSelect

## Question
Given an integer array `nums` and an integer `k`, return the `k`th largest element in the array.

Note that it is the `k`th largest element in the sorted order, not the `k`th distinct element.

## Example Input/Output
**Input**: nums = [3,2,1,5,6,4], k = 2  
**Output**: 5

**Input**: nums = [3,2,3,1,2,4,5,5,6], k = 4  
**Output**: 4

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Sorting
1. Sort the array in descending order
2. Return the element at index k-1

### Approach 2: Min Heap (Priority Queue)
1. Create a min heap of size k
2. Iterate through the array:
   - If the heap size is less than k, add the current element
   - If the current element is larger than the smallest element in the heap, remove the smallest element and add the current element
3. The top element of the heap is the kth largest element

### Approach 3: QuickSelect (Optimal)
1. Use the QuickSelect algorithm, which is based on the partition scheme of QuickSort
2. In each step, choose a pivot and partition the array
3. If the pivot is at position k-1 (0-indexed), return the pivot
4. If the pivot is at a position less than k-1, search in the right subarray
5. If the pivot is at a position greater than k-1, search in the left subarray

## Solution Code
```java
// Approach 1: Sorting (O(n log n))
public int findKthLargest(int[] nums, int k) {
    Arrays.sort(nums);
    return nums[nums.length - k];
}
```

```java
// Approach 2: Min Heap (O(n log k))
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int num : nums) {
        minHeap.offer(num);
        
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }
    
    return minHeap.peek();
}
```

```java
// Approach 3: QuickSelect (Average O(n), Worst O(n²))
public int findKthLargest(int[] nums, int k) {
    // Convert to 0-indexed
    k = nums.length - k;
    
    return quickSelect(nums, 0, nums.length - 1, k);
}

private int quickSelect(int[] nums, int left, int right, int k) {
    if (left == right) {
        return nums[left];
    }
    
    // Choose a random pivot to avoid worst-case performance
    int pivotIndex = left + (int) (Math.random() * (right - left + 1));
    pivotIndex = partition(nums, left, right, pivotIndex);
    
    if (pivotIndex == k) {
        return nums[k];
    } else if (pivotIndex < k) {
        return quickSelect(nums, pivotIndex + 1, right, k);
    } else {
        return quickSelect(nums, left, pivotIndex - 1, k);
    }
}

private int partition(int[] nums, int left, int right, int pivotIndex) {
    int pivotValue = nums[pivotIndex];
    
    // Move pivot to the end
    swap(nums, pivotIndex, right);
    
    // Move all elements smaller than pivot to the left
    int storeIndex = left;
    for (int i = left; i < right; i++) {
        if (nums[i] < pivotValue) {
            swap(nums, i, storeIndex);
            storeIndex++;
        }
    }
    
    // Move pivot to its final place
    swap(nums, storeIndex, right);
    
    return storeIndex;
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
``` 