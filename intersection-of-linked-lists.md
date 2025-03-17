# Intersection of Two Linked Lists

**Difficulty**: Easy  
**Tags**: Hash Table, Linked List, Two Pointers

## Question
Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return `null`.

Note that the linked lists must retain their original structure after the function returns.

## Example Input/Output
**Input**: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3  
**Output**: Intersected at '8'  
**Explanation**: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.

**Input**: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1  
**Output**: Intersected at '2'  
**Explanation**: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.

**Input**: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2  
**Output**: No intersection  
**Explanation**: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values. The two lists do not intersect, so return null.

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Hash Set
1. Traverse the first linked list and store each node in a hash set.
2. Traverse the second linked list and check if any node is already in the hash set.
3. If a node is found in the hash set, it's the intersection point.

### Approach 2: Two Pointers
1. Calculate the lengths of both linked lists.
2. Find the difference in lengths.
3. Move the pointer of the longer list forward by the difference.
4. Then move both pointers forward in tandem until they meet.

### Approach 3: Two Pointers (Elegant Solution)
1. Create two pointers, one for each list.
2. Traverse both lists simultaneously.
3. When a pointer reaches the end of its list, redirect it to the head of the other list.
4. Continue until the pointers meet or both become null.

## Solution Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
// Approach 1: Hash Set
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    Set<ListNode> nodesInB = new HashSet<>();
    
    // Add all nodes from list B to the hash set
    ListNode currentB = headB;
    while (currentB != null) {
        nodesInB.add(currentB);
        currentB = currentB.next;
    }
    
    // Check if any node in list A is in the hash set
    ListNode currentA = headA;
    while (currentA != null) {
        if (nodesInB.contains(currentA)) {
            return currentA;
        }
        currentA = currentA.next;
    }
    
    return null;
}
```

```java
// Approach 2: Two Pointers (Length Difference)
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // Calculate lengths of both lists
    int lengthA = getLength(headA);
    int lengthB = getLength(headB);
    
    // Determine which list is longer
    ListNode ptrA = headA;
    ListNode ptrB = headB;
    
    // Move the pointer of the longer list forward
    if (lengthA > lengthB) {
        for (int i = 0; i < lengthA - lengthB; i++) {
            ptrA = ptrA.next;
        }
    } else {
        for (int i = 0; i < lengthB - lengthA; i++) {
            ptrB = ptrB.next;
        }
    }
    
    // Move both pointers until they meet
    while (ptrA != ptrB) {
        ptrA = ptrA.next;
        ptrB = ptrB.next;
    }
    
    return ptrA; // Will be null if no intersection
}

private int getLength(ListNode head) {
    int length = 0;
    ListNode current = head;
    while (current != null) {
        length++;
        current = current.next;
    }
    return length;
}
```

```java
// Approach 3: Two Pointers (Elegant Solution)
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) {
        return null;
    }
    
    ListNode ptrA = headA;
    ListNode ptrB = headB;
    
    // If the lists have different lengths, the pointers will eventually
    // align after they've both traversed both lists
    while (ptrA != ptrB) {
        // When ptrA reaches the end, redirect it to headB
        ptrA = (ptrA == null) ? headB : ptrA.next;
        
        // When ptrB reaches the end, redirect it to headA
        ptrB = (ptrB == null) ? headA : ptrB.next;
    }
    
    return ptrA; // Will be null if no intersection
}
``` 