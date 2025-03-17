# Merge k Sorted Lists

**Difficulty**: Hard  
**Tags**: Linked List, Divide and Conquer, Heap, Merge Sort

## Question
You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

## Example Input/Output
**Input**: lists = [[1,4,5],[1,3,4],[2,6]]  
**Output**: [1,1,2,3,4,4,5,6]  
**Explanation**: The linked-lists are:
```
[
  1->4->5,
  1->3->4,
  2->6
]
```
Merging them into one sorted list:
```
1->1->2->3->4->4->5->6
```

**Input**: lists = []  
**Output**: []

**Input**: lists = [[]]  
**Output**: []

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Using a Min Heap (Priority Queue)
1. Create a min heap to store the heads of all k linked lists
2. Initialize the heap with the head of each non-empty list
3. While the heap is not empty:
   - Remove the smallest node from the heap
   - Add it to the result list
   - If the removed node has a next node, add that to the heap
4. Return the head of the result list

### Approach 2: Divide and Conquer
1. Recursively merge pairs of lists until only one list remains
2. For merging two sorted lists, use the standard merge algorithm from merge sort

## Solution Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

// Approach 1: Using a Min Heap
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }
    
    // Create a min heap based on node values
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);
    
    // Add the head of each non-empty list to the heap
    for (ListNode list : lists) {
        if (list != null) {
            minHeap.offer(list);
        }
    }
    
    // Create a dummy head for the result list
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    // Process nodes in sorted order
    while (!minHeap.isEmpty()) {
        // Get the smallest node
        ListNode node = minHeap.poll();
        current.next = node;
        current = current.next;
        
        // If this node has a next node, add it to the heap
        if (node.next != null) {
            minHeap.offer(node.next);
        }
    }
    
    return dummy.next;
}
```

```java
// Approach 2: Divide and Conquer
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }
    
    return divideAndConquer(lists, 0, lists.length - 1);
}

private ListNode divideAndConquer(ListNode[] lists, int start, int end) {
    if (start == end) {
        return lists[start];
    }
    
    if (start + 1 == end) {
        return mergeTwoLists(lists[start], lists[end]);
    }
    
    int mid = start + (end - start) / 2;
    ListNode left = divideAndConquer(lists, start, mid);
    ListNode right = divideAndConquer(lists, mid + 1, end);
    
    return mergeTwoLists(left, right);
}

private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    // Attach the remaining nodes
    if (l1 != null) {
        current.next = l1;
    } else {
        current.next = l2;
    }
    
    return dummy.next;
}
``` 