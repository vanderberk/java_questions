# Word Break

**Difficulty**: Medium  
**Tags**: Hash Table, String, Dynamic Programming, Trie, Memoization

## Question
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

## Example Input/Output
**Input**: s = "leetcode", wordDict = ["leet","code"]  
**Output**: true  
**Explanation**: Return true because "leetcode" can be segmented as "leet code".

**Input**: s = "applepenapple", wordDict = ["apple","pen"]  
**Output**: true  
**Explanation**: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.

**Input**: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]  
**Output**: false  
**Explanation**: The string cannot be segmented into a sequence of dictionary words.

## Solution Algorithm
This problem can be solved using dynamic programming. We'll explore multiple approaches:

### Approach 1: Dynamic Programming (Bottom-Up)
1. Create a boolean DP array `dp` of length `n+1` where `n` is the length of the string `s`.
2. `dp[i]` will be `true` if the substring `s[0...i-1]` can be segmented into dictionary words.
3. Initialize `dp[0] = true` as an empty string can always be segmented.
4. For each position `i` from 1 to `n`:
   - For each position `j` from 0 to `i-1`:
     - If `dp[j]` is `true` and the substring `s[j...i-1]` is in the dictionary, set `dp[i]` to `true`.
5. Return `dp[n]`.

### Approach 2: Dynamic Programming with Memoization (Top-Down)
1. Use a recursive function with memoization to check if a substring can be segmented.
2. For each position in the string, try all possible words from the dictionary.
3. If a word matches the current substring, recursively check if the remaining substring can be segmented.
4. Use memoization to avoid redundant calculations.

### Approach 3: BFS
1. Use BFS to explore all possible segmentations.
2. Start from position 0 and try all words that match the current substring.
3. If a word matches, add the end position of the word to the queue.
4. If we reach the end of the string, return `true`.

## Solution Code
```java
// Approach 1: Dynamic Programming (Bottom-Up)
public boolean wordBreak(String s, List<String> wordDict) {
    int n = s.length();
    boolean[] dp = new boolean[n + 1];
    dp[0] = true; // Empty string can be segmented
    
    // Convert wordDict to a HashSet for O(1) lookups
    Set<String> wordSet = new HashSet<>(wordDict);
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && wordSet.contains(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[n];
}
```

```java
// Approach 2: Dynamic Programming with Memoization (Top-Down)
public boolean wordBreak(String s, List<String> wordDict) {
    return wordBreakMemo(s, new HashSet<>(wordDict), 0, new Boolean[s.length()]);
}

private boolean wordBreakMemo(String s, Set<String> wordDict, int start, Boolean[] memo) {
    if (start == s.length()) {
        return true;
    }
    
    if (memo[start] != null) {
        return memo[start];
    }
    
    for (int end = start + 1; end <= s.length(); end++) {
        if (wordDict.contains(s.substring(start, end)) && wordBreakMemo(s, wordDict, end, memo)) {
            memo[start] = true;
            return true;
        }
    }
    
    memo[start] = false;
    return false;
}
```

```java
// Approach 3: BFS
public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    Queue<Integer> queue = new LinkedList<>();
    boolean[] visited = new boolean[s.length()];
    
    queue.offer(0);
    
    while (!queue.isEmpty()) {
        int start = queue.poll();
        
        if (start == s.length()) {
            return true;
        }
        
        if (visited[start]) {
            continue;
        }
        
        visited[start] = true;
        
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordSet.contains(s.substring(start, end))) {
                queue.offer(end);
            }
        }
    }
    
    return false;
}
```

```java
// Approach 4: Using Trie for more efficient word lookup
public boolean wordBreak(String s, List<String> wordDict) {
    // Build Trie
    TrieNode root = new TrieNode();
    for (String word : wordDict) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (node.children[c - 'a'] == null) {
                node.children[c - 'a'] = new TrieNode();
            }
            node = node.children[c - 'a'];
        }
        node.isWord = true;
    }
    
    int n = s.length();
    boolean[] dp = new boolean[n + 1];
    dp[0] = true;
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && search(root, s, j, i)) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[n];
}

private boolean search(TrieNode root, String s, int start, int end) {
    TrieNode node = root;
    for (int i = start; i < end; i++) {
        char c = s.charAt(i);
        if (node.children[c - 'a'] == null) {
            return false;
        }
        node = node.children[c - 'a'];
    }
    return node.isWord;
}

class TrieNode {
    TrieNode[] children;
    boolean isWord;
    
    public TrieNode() {
        children = new TrieNode[26];
        isWord = false;
    }
}
``` 