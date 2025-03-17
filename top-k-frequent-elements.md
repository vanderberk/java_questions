# Top K Frequent Elements

**Difficulty**: Medium  
**Tags**: Array, Hash Table, Divide and Conquer, Sorting, Heap, Bucket Sort, Counting, Quickselect

## Question
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

## Example Input/Output
**Input**: nums = [1,1,1,2,2,3], k = 2  
**Output**: [1,2]

**Input**: nums = [1], k = 1  
**Output**: [1]

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Hash Map + Heap
1. Use a hash map to count the frequency of each element in the array.
2. Use a min heap of size k to keep track of the k most frequent elements.
3. Iterate through the hash map and add elements to the heap.
4. If the heap size exceeds k, remove the element with the minimum frequency.
5. Finally, extract all elements from the heap to get the result.

### Approach 2: Hash Map + Bucket Sort
1. Use a hash map to count the frequency of each element in the array.
2. Create a list of buckets where bucket[i] contains all elements that appear i times.
3. Iterate through the buckets from the highest frequency to the lowest.
4. Add elements to the result list until we have k elements.

### Approach 3: Hash Map + Quickselect
1. Use a hash map to count the frequency of each element in the array.
2. Create a list of unique elements.
3. Use the quickselect algorithm to find the k elements with the highest frequencies.

## Solution Code
```java
// Approach 1: Hash Map + Heap
public int[] topKFrequent(int[] nums, int k) {
    // Count the frequency of each element
    Map<Integer, Integer> frequencyMap = new HashMap<>();
    for (int num : nums) {
        frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
    }
    
    // Use a min heap to keep track of the k most frequent elements
    PriorityQueue<Integer> minHeap = new PriorityQueue<>((a, b) -> 
        frequencyMap.get(a) - frequencyMap.get(b));
    
    // Add elements to the heap
    for (int num : frequencyMap.keySet()) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll(); // Remove the element with the minimum frequency
        }
    }
    
    // Extract elements from the heap to get the result
    int[] result = new int[k];
    for (int i = k - 1; i >= 0; i--) {
        result[i] = minHeap.poll();
    }
    
    return result;
}
```

```java
// Approach 2: Hash Map + Bucket Sort
public int[] topKFrequent(int[] nums, int k) {
    // Count the frequency of each element
    Map<Integer, Integer> frequencyMap = new HashMap<>();
    for (int num : nums) {
        frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
    }
    
    // Create buckets where bucket[i] contains all elements that appear i times
    List<Integer>[] buckets = new List[nums.length + 1];
    for (int i = 0; i < buckets.length; i++) {
        buckets[i] = new ArrayList<>();
    }
    
    for (int num : frequencyMap.keySet()) {
        int frequency = frequencyMap.get(num);
        buckets[frequency].add(num);
    }
    
    // Extract the k most frequent elements
    int[] result = new int[k];
    int index = 0;
    
    for (int i = buckets.length - 1; i >= 0 && index < k; i--) {
        for (int num : buckets[i]) {
            result[index++] = num;
            if (index == k) {
                break;
            }
        }
    }
    
    return result;
}
```

```java
// Approach 3: Hash Map + Quickselect
public int[] topKFrequent(int[] nums, int k) {
    // Count the frequency of each element
    Map<Integer, Integer> frequencyMap = new HashMap<>();
    for (int num : nums) {
        frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
    }
    
    // Create a list of unique elements
    List<Integer> uniqueNums = new ArrayList<>(frequencyMap.keySet());
    
    // Use quickselect to find the k elements with the highest frequencies
    int left = 0, right = uniqueNums.size() - 1;
    int targetIndex = uniqueNums.size() - k;
    
    while (left < right) {
        int pivotIndex = partition(uniqueNums, left, right, frequencyMap);
        
        if (pivotIndex == targetIndex) {
            break;
        } else if (pivotIndex < targetIndex) {
            left = pivotIndex + 1;
        } else {
            right = pivotIndex - 1;
        }
    }
    
    // Extract the k most frequent elements
    int[] result = new int[k];
    for (int i = 0; i < k; i++) {
        result[i] = uniqueNums.get(uniqueNums.size() - 1 - i);
    }
    
    return result;
}

private int partition(List<Integer> nums, int left, int right, Map<Integer, Integer> frequencyMap) {
    int pivot = frequencyMap.get(nums.get(right));
    int i = left;
    
    for (int j = left; j < right; j++) {
        if (frequencyMap.get(nums.get(j)) <= pivot) {
            Collections.swap(nums, i, j);
            i++;
        }
    }
    
    Collections.swap(nums, i, right);
    return i;
}
``` 