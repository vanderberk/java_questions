# Word Ladder

**Difficulty**: Hard  
**Tags**: Breadth-First Search (BFS), Hash Table, String

## Question
A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` must exist in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return the **number of words** in the **shortest transformation sequence** from `beginWord` to `endWord`, or `0` if no such sequence exists.

## Example Input/Output
**Example 1:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> "cog", which is 5 words long.
```

**Example 2:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, so there is no valid transformation sequence.
```

## Constraints:
- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are **unique**.

## Solution Algorithm
This problem can be solved using Breadth-First Search (BFS) to find the shortest path from `beginWord` to `endWord`. Here's the approach:

1. Create a set from the `wordList` for O(1) lookups.
2. Check if `endWord` is in the set. If not, return 0.
3. Initialize a queue for BFS and add `beginWord` to it.
4. Initialize a visited set to keep track of words we've already processed.
5. Start BFS:
   - For each word in the queue, try changing each character to all possible letters (a-z).
   - If the new word is in the word list and hasn't been visited, add it to the queue.
   - If the new word is the `endWord`, return the current level + 1.
6. If BFS completes without finding `endWord`, return 0.

### Optimizations:
- Bidirectional BFS: Start BFS from both `beginWord` and `endWord` simultaneously.
- Pattern-based approach: Instead of trying all 26 letters, create patterns like "*ot" for "hot" and use them as keys in a map.

## Solution Code
```java
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // Convert wordList to a HashSet for O(1) lookups
        Set<String> wordSet = new HashSet<>(wordList);
        
        // If endWord is not in the dictionary, no solution exists
        if (!wordSet.contains(endWord)) {
            return 0;
        }
        
        // Queue for BFS
        Queue<String> queue = new LinkedList<>();
        queue.offer(beginWord);
        
        // Set to keep track of visited words
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        
        int level = 1; // beginWord counts as 1
        
        // BFS
        while (!queue.isEmpty()) {
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                String currentWord = queue.poll();
                
                // Try changing each character of the current word
                char[] wordChars = currentWord.toCharArray();
                
                for (int j = 0; j < wordChars.length; j++) {
                    char originalChar = wordChars[j];
                    
                    // Try all possible characters
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == originalChar) {
                            continue;
                        }
                        
                        wordChars[j] = c;
                        String newWord = new String(wordChars);
                        
                        // If we found the endWord, return the level + 1
                        if (newWord.equals(endWord)) {
                            return level + 1;
                        }
                        
                        // If the new word is in the dictionary and hasn't been visited
                        if (wordSet.contains(newWord) && !visited.contains(newWord)) {
                            queue.offer(newWord);
                            visited.add(newWord);
                        }
                    }
                    
                    // Restore the original character
                    wordChars[j] = originalChar;
                }
            }
            
            level++;
        }
        
        return 0; // No transformation sequence found
    }
}
```

```java
// Optimized solution using bidirectional BFS
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // Convert wordList to a HashSet for O(1) lookups
        Set<String> wordSet = new HashSet<>(wordList);
        
        // If endWord is not in the dictionary, no solution exists
        if (!wordSet.contains(endWord)) {
            return 0;
        }
        
        // Bidirectional BFS
        Set<String> beginSet = new HashSet<>();
        Set<String> endSet = new HashSet<>();
        beginSet.add(beginWord);
        endSet.add(endWord);
        
        Set<String> visited = new HashSet<>();
        
        int length = 1;
        
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            // Always expand the smaller set for efficiency
            if (beginSet.size() > endSet.size()) {
                Set<String> temp = beginSet;
                beginSet = endSet;
                endSet = temp;
            }
            
            Set<String> nextLevel = new HashSet<>();
            
            for (String word : beginSet) {
                char[] wordChars = word.toCharArray();
                
                for (int i = 0; i < wordChars.length; i++) {
                    char originalChar = wordChars[i];
                    
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == originalChar) {
                            continue;
                        }
                        
                        wordChars[i] = c;
                        String newWord = new String(wordChars);
                        
                        // If the other direction contains this word, we've found a path
                        if (endSet.contains(newWord)) {
                            return length + 1;
                        }
                        
                        if (wordSet.contains(newWord) && !visited.contains(newWord)) {
                            nextLevel.add(newWord);
                            visited.add(newWord);
                        }
                    }
                    
                    wordChars[i] = originalChar;
                }
            }
            
            beginSet = nextLevel;
            length++;
        }
        
        return 0; // No transformation sequence found
    }
}
```

```java
// Pattern-based approach for optimization
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // If endWord is not in the wordList, no solution exists
        if (!wordList.contains(endWord)) {
            return 0;
        }
        
        int L = beginWord.length();
        
        // Dictionary to hold all possible intermediate words
        Map<String, List<String>> allComboDict = new HashMap<>();
        
        // Preprocess wordList to create a map of all generic word states
        wordList.forEach(word -> {
            for (int i = 0; i < L; i++) {
                // Key is the generic word state
                // Value is a list of words that can be formed by that generic state
                String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);
                List<String> transformations = allComboDict.getOrDefault(newWord, new ArrayList<>());
                transformations.add(word);
                allComboDict.put(newWord, transformations);
            }
        });
        
        // Queue for BFS
        Queue<Pair<String, Integer>> queue = new LinkedList<>();
        queue.add(new Pair<>(beginWord, 1));
        
        // Visited to avoid processing same word
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        
        while (!queue.isEmpty()) {
            Pair<String, Integer> node = queue.remove();
            String word = node.getKey();
            int level = node.getValue();
            
            for (int i = 0; i < L; i++) {
                // Create generic word state
                String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);
                
                // Get all words that share the same intermediate state
                for (String adjacentWord : allComboDict.getOrDefault(newWord, new ArrayList<>())) {
                    // If we found the endWord, return the level
                    if (adjacentWord.equals(endWord)) {
                        return level + 1;
                    }
                    
                    if (!visited.contains(adjacentWord)) {
                        visited.add(adjacentWord);
                        queue.add(new Pair<>(adjacentWord, level + 1));
                    }
                }
            }
        }
        
        return 0;
    }
    
    // Simple Pair class for the queue
    private class Pair<K, V> {
        private K key;
        private V value;
        
        public Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }
        
        public K getKey() {
            return key;
        }
        
        public V getValue() {
            return value;
        }
    }
}
``` 