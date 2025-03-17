# Linked List Cycle

**Difficulty**: Easy  
**Tags**: Linked List, Two Pointers, Hash Table

## Question
Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

## Example Input/Output
**Input**: head = [3,2,0,-4], pos = 1  
**Output**: true  
**Explanation**: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Input**: head = [1,2], pos = 0  
**Output**: true  
**Explanation**: There is a cycle in the linked list, where the tail connects to the 0th node.

**Input**: head = [1], pos = -1  
**Output**: false  
**Explanation**: There is no cycle in the linked list.

## Solution Algorithm
There are two main approaches to solve this problem:

### Approach 1: Hash Set
1. Create a hash set to store nodes we've visited.
2. Traverse the linked list:
   - If the current node is null, we've reached the end of the list, so return false.
   - If the current node is already in the hash set, we've detected a cycle, so return true.
   - Otherwise, add the current node to the hash set and move to the next node.

### Approach 2: Floyd's Cycle-Finding Algorithm (Tortoise and Hare)
1. Use two pointers, slow and fast, both starting at the head.
2. The slow pointer moves one step at a time, while the fast pointer moves two steps at a time.
3. If there is a cycle, the fast pointer will eventually catch up to the slow pointer.
4. If there is no cycle, the fast pointer will reach the end of the list.

## Solution Code
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

// Approach 1: Hash Set
public boolean hasCycle(ListNode head) {
    Set<ListNode> visited = new HashSet<>();
    
    ListNode current = head;
    while (current != null) {
        if (visited.contains(current)) {
            return true;
        }
        
        visited.add(current);
        current = current.next;
    }
    
    return false;
}
```

```java
// Approach 2: Floyd's Cycle-Finding Algorithm (Tortoise and Hare)
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;          // Move one step
        fast = fast.next.next;     // Move two steps
        
        if (slow == fast) {
            return true;  // Cycle detected
        }
    }
    
    return false;  // Reached the end, no cycle
}
``` 