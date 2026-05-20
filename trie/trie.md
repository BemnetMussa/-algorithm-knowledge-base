# Trie (Prefix Tree)

## One-sentence definition
A Trie is a tree‑based data structure that stores a set of strings (usually words) by sharing common prefixes, allowing fast prefix search and word lookup in O(L) time where L is the length of the string.

## When to use it (use cases)
- Autocomplete / search suggestions (e.g., typing “app” shows “apple”, “application”).
- Spell checking / dictionary validation.
- IP routing (longest prefix matching).
- Word games (Boggle, Scrabble, word search).
- Finding all words with a given prefix.
- As a space‑efficient alternative to hash sets when many strings share prefixes.

## Step-by-step explanation (plain language, no code first)

1. **Node structure** – each node contains:
   - An array (or dictionary) of child nodes (one per possible character, often 26 for lowercase letters).
   - A boolean flag indicating whether this node represents the end of a valid word.

2. **Insert** – start at the root. For each character in the word, follow the child pointer (or create it if missing). After the last character, mark the node as end‑of‑word.

3. **Search** – start at root. For each character in the word, follow the child pointer. If at any step the pointer is missing → word not found. At the end, check if the node is marked as end‑of‑word.

4. **Prefix search** – similar to search, but you don't need the end‑of‑word flag; just check if all characters exist along the path.

> **Analogy:** A family tree of letters. The root has children for each possible first letter. Each of those has children for the second letter, etc. Any path from root to a marked node spells a valid word.

## Templates (Python)

### Trie node with dictionary (flexible for any characters)
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end = True
    
    def search(self, word: str) -> bool:
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end
    
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for ch in prefix:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return True
```

### Trie with array of 26 children (lowercase English letters only)
```python
class TrieNodeArray:
    def __init__(self):
        self.children = [None] * 26
        self.is_end = False

class TrieArray:
    def __init__(self):
        self.root = TrieNodeArray()
    
    def _char_index(self, ch):
        return ord(ch) - ord('a')
    
    def insert(self, word: str) -> None:
        node = self.root
        for ch in word:
            idx = self._char_index(ch)
            if not node.children[idx]:
                node.children[idx] = TrieNodeArray()
            node = node.children[idx]
        node.is_end = True
    
    def search(self, word: str) -> bool:
        node = self.root
        for ch in word:
            idx = self._char_index(ch)
            if not node.children[idx]:
                return False
            node = node.children[idx]
        return node.is_end
    
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for ch in prefix:
            idx = self._char_index(ch)
            if not node.children[idx]:
                return False
            node = node.children[idx]
        return True
```

## Time & space complexity (Big O)
- **Time:** `O(L)` for insert, search, and startsWith, where L = length of the word/prefix. This is independent of the number of stored words.
- **Space:** `O(total characters across all words)` – each character stored once per unique path. Worst‑case (no shared prefixes): `O(total characters)`. In practice, trie is more space‑heavy than a hash set.

## Practice questions (concept check)
1. How does a Trie save space compared to storing all words separately?
2. What would you change to support delete operation?
3. When would you use a hash set instead of a Trie?

<details>
<summary>Answers</summary>

1. Common prefixes are stored only once. Words like “car”, “cart”, “carpet” share “car”, reducing total storage.

2. To delete, you traverse to the node, mark `is_end = False`, and optionally remove child pointers that are no longer part of any word (you can clean up recursively from the bottom).

3. Hash sets are faster for exact lookups (O(1) average) and use less memory for sparse data. Use Trie when you need prefix operations (autocomplete, prefix matching) or when many words share long prefixes.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) – the standard problem.
2. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/) – Trie with wildcard `.` matching.
3. [Word Search II](https://leetcode.com/problems/word-search-ii/) – Trie + DFS.
4. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) – can be solved with a Trie.
5. [Replace Words](https://leetcode.com/problems/replace-words/) – Trie for prefix replacement.

## One thing that was confusing to me
I initially thought the root node could hold a character. But the root is empty – it just points to the first letters of all words. That took me a while to accept. Also, the distinction between search (needs `is_end = True`) and startsWith (just path existence) is subtle but critical.

