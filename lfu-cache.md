# LFU Cache

**Difficulty**: Hard  
**Tags**: Hash Table, Linked List, Design, Doubly-Linked List

## Question
Design and implement a data structure for a Least Frequently Used (LFU) cache.

Implement the `LFUCache` class:
- `LFUCache(int capacity)` Initializes the object with the `capacity` of the data structure.
- `int get(int key)` Gets the value of the `key` if the `key` exists in the cache. Otherwise, returns -1.
- `void put(int key, int value)` Update the value of the `key` if present, or inserts the `key` if not already present. When the cache reaches its `capacity`, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key would be invalidated.

To determine the least frequently used key, a use counter is maintained for each key in the cache. The key with the smallest use counter is the least frequently used key.

When a key is first inserted into the cache, its use counter is set to 1 (due to the put operation). The use counter for a key in the cache is incremented either a get or put operation is called on it.

The functions `get` and `put` must each run in O(1) average time complexity.

## Example Input/Output
```
Input:
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
Output:
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

Explanation:
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3
```

## Solution Algorithm
To implement an LFU cache with O(1) time complexity for both `get` and `put` operations, we need to use a combination of hash maps and doubly linked lists:

1. Use a hash map to store key-value pairs for O(1) access.
2. Use another hash map to store the frequency of each key.
3. Use a third hash map to store keys with the same frequency in a doubly linked list (to maintain order for LRU tiebreaker).
4. Keep track of the minimum frequency to quickly find the least frequently used key.

### Key Components:
- `keyToVal`: Maps keys to their values
- `keyToFreq`: Maps keys to their frequencies
- `freqToKeys`: Maps frequencies to a doubly linked list of keys with that frequency
- `minFreq`: Tracks the minimum frequency

### Operations:
1. `get(key)`:
   - If the key doesn't exist, return -1.
   - Increment the frequency of the key.
   - Move the key from its current frequency list to the next frequency list.
   - Update `minFreq` if necessary.
   - Return the value.

2. `put(key, value)`:
   - If the key already exists, update its value and handle it like a `get` operation.
   - If the cache is full, remove the least frequently used key.
   - Add the new key with frequency 1.
   - Update `minFreq` to 1.

## Solution Code
```java
import java.util.*;

class LFUCache {
    private int capacity;
    private int minFreq;
    private Map<Integer, Integer> keyToVal;
    private Map<Integer, Integer> keyToFreq;
    private Map<Integer, LinkedHashSet<Integer>> freqToKeys;
    
    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.minFreq = 0;
        this.keyToVal = new HashMap<>();
        this.keyToFreq = new HashMap<>();
        this.freqToKeys = new HashMap<>();
    }
    
    public int get(int key) {
        if (!keyToVal.containsKey(key)) {
            return -1;
        }
        
        // Increment frequency
        int freq = keyToFreq.get(key);
        keyToFreq.put(key, freq + 1);
        
        // Remove key from current frequency list
        freqToKeys.get(freq).remove(key);
        
        // If current frequency list is empty and it's the minimum frequency,
        // increment the minimum frequency
        if (freq == minFreq && freqToKeys.get(freq).isEmpty()) {
            minFreq++;
        }
        
        // Add key to the new frequency list
        freqToKeys.computeIfAbsent(freq + 1, k -> new LinkedHashSet<>()).add(key);
        
        return keyToVal.get(key);
    }
    
    public void put(int key, int value) {
        if (capacity <= 0) {
            return;
        }
        
        // If key exists, update value and frequency
        if (keyToVal.containsKey(key)) {
            keyToVal.put(key, value);
            get(key); // This will update the frequency
            return;
        }
        
        // If cache is full, remove the least frequently used key
        if (keyToVal.size() >= capacity) {
            int evictKey = freqToKeys.get(minFreq).iterator().next();
            freqToKeys.get(minFreq).remove(evictKey);
            keyToVal.remove(evictKey);
            keyToFreq.remove(evictKey);
        }
        
        // Add new key with frequency 1
        keyToVal.put(key, value);
        keyToFreq.put(key, 1);
        freqToKeys.computeIfAbsent(1, k -> new LinkedHashSet<>()).add(key);
        minFreq = 1;
    }
}
```

```java
// Alternative implementation with a Node class for clarity
class LFUCache {
    class Node {
        int key;
        int value;
        int frequency;
        
        public Node(int key, int value) {
            this.key = key;
            this.value = value;
            this.frequency = 1;
        }
    }
    
    private int capacity;
    private int minFrequency;
    private Map<Integer, Node> cache;
    private Map<Integer, LinkedHashSet<Integer>> frequencyMap;
    
    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.minFrequency = 0;
        this.cache = new HashMap<>();
        this.frequencyMap = new HashMap<>();
    }
    
    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        
        // Get the node and update its frequency
        Node node = cache.get(key);
        updateFrequency(node);
        
        return node.value;
    }
    
    public void put(int key, int value) {
        if (capacity <= 0) {
            return;
        }
        
        // If key exists, update value and frequency
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            node.value = value;
            updateFrequency(node);
            return;
        }
        
        // If cache is full, remove the least frequently used key
        if (cache.size() >= capacity) {
            int evictKey = frequencyMap.get(minFrequency).iterator().next();
            frequencyMap.get(minFrequency).remove(evictKey);
            cache.remove(evictKey);
        }
        
        // Add new key with frequency 1
        Node newNode = new Node(key, value);
        cache.put(key, newNode);
        minFrequency = 1;
        frequencyMap.computeIfAbsent(1, k -> new LinkedHashSet<>()).add(key);
    }
    
    private void updateFrequency(Node node) {
        int oldFrequency = node.frequency;
        
        // Remove from old frequency list
        frequencyMap.get(oldFrequency).remove(node.key);
        
        // If current frequency list is empty and it's the minimum frequency,
        // increment the minimum frequency
        if (oldFrequency == minFrequency && frequencyMap.get(oldFrequency).isEmpty()) {
            minFrequency++;
        }
        
        // Increment node's frequency
        node.frequency++;
        
        // Add to new frequency list
        frequencyMap.computeIfAbsent(node.frequency, k -> new LinkedHashSet<>()).add(node.key);
    }
}
```

```java
// Implementation with more detailed comments
class LFUCache {
    // Key-value storage
    private Map<Integer, Integer> keyToValue;
    // Key to frequency mapping
    private Map<Integer, Integer> keyToFrequency;
    // Frequency to ordered set of keys (ordered by insertion time for LRU tiebreaker)
    private Map<Integer, LinkedHashSet<Integer>> frequencyToKeys;
    
    private int capacity;
    private int minFrequency;
    
    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.keyToValue = new HashMap<>();
        this.keyToFrequency = new HashMap<>();
        this.frequencyToKeys = new HashMap<>();
        this.minFrequency = 0;
    }
    
    public int get(int key) {
        // If key doesn't exist, return -1
        if (!keyToValue.containsKey(key)) {
            return -1;
        }
        
        // Get current frequency and increment it
        int currentFrequency = keyToFrequency.get(key);
        int newFrequency = currentFrequency + 1;
        
        // Update frequency mapping
        keyToFrequency.put(key, newFrequency);
        
        // Remove key from current frequency set
        frequencyToKeys.get(currentFrequency).remove(key);
        
        // If the current frequency set is now empty and it was the minimum frequency,
        // increment the minimum frequency
        if (currentFrequency == minFrequency && frequencyToKeys.get(currentFrequency).isEmpty()) {
            minFrequency++;
        }
        
        // Add key to the new frequency set
        frequencyToKeys.computeIfAbsent(newFrequency, k -> new LinkedHashSet<>()).add(key);
        
        // Return the value
        return keyToValue.get(key);
    }
    
    public void put(int key, int value) {
        // If capacity is 0, do nothing
        if (capacity <= 0) {
            return;
        }
        
        // If key already exists, update its value and frequency
        if (keyToValue.containsKey(key)) {
            keyToValue.put(key, value);
            get(key); // This will update the frequency
            return;
        }
        
        // If cache is full, remove the least frequently used key
        if (keyToValue.size() >= capacity) {
            // Get the first key from the minimum frequency set (least recently used among least frequent)
            int keyToRemove = frequencyToKeys.get(minFrequency).iterator().next();
            
            // Remove it from all maps
            frequencyToKeys.get(minFrequency).remove(keyToRemove);
            keyToValue.remove(keyToRemove);
            keyToFrequency.remove(keyToRemove);
        }
        
        // Add the new key with frequency 1
        keyToValue.put(key, value);
        keyToFrequency.put(key, 1);
        frequencyToKeys.computeIfAbsent(1, k -> new LinkedHashSet<>()).add(key);
        
        // Set minimum frequency to 1 since we just added a new key
        minFrequency = 1;
    }
}
``` 