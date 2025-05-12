# String Algorithms

A collection of essential string algorithms and techniques commonly used in programming.

## String Matching

### Naive String Matching

The simplest approach to find all occurrences of a pattern in a text.

```cpp
vector<int> naiveSearch(string text, string pattern) {
    vector<int> positions;
    int n = text.length();
    int m = pattern.length();
    
    for (int i = 0; i <= n - m; i++) {
        int j;
        for (j = 0; j < m; j++) {
            if (text[i + j] != pattern[j])
                break;
        }
        
        if (j == m) // Pattern found
            positions.push_back(i);
    }
    
    return positions;
}
```

**Time Complexity:** O(n * m) where n is the text length and m is the pattern length

### KMP Algorithm

An efficient string matching algorithm that uses preprocessing to avoid unnecessary comparisons.

```cpp
vector<int> kmpSearch(string text, string pattern) {
    int n = text.length();
    int m = pattern.length();
    vector<int> positions;
    
    // Preprocessing: building the LPS array
    vector<int> lps(m, 0);
    int len = 0, i = 1;
    
    while (i < m) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    
    // Searching
    i = 0;
    int j = 0;
    while (i < n) {
        if (pattern[j] == text[i]) {
            i++; j++;
        }
        
        if (j == m) {
            positions.push_back(i - j);
            j = lps[j - 1];
        } else if (i < n && pattern[j] != text[i]) {
            if (j != 0)
                j = lps[j - 1];
            else
                i++;
        }
    }
    
    return positions;
}
```

**Time Complexity:** O(n + m)

## Common String Operations

### Palindrome Check

```cpp
bool isPalindrome(string s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s[left] != s[right])
            return false;
        left++;
        right--;
    }
    
    return true;
}
```

### String Reversal

```cpp
string reverseString(string s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        swap(s[left], s[right]);
        left++;
        right--;
    }
    
    return s;
}
```

### Anagram Check

```cpp
bool isAnagram(string s1, string s2) {
    if (s1.length() != s2.length())
        return false;
        
    vector<int> count(26, 0);
    
    for (int i = 0; i < s1.length(); i++) {
        count[s1[i] - 'a']++;
        count[s2[i] - 'a']--;
    }
    
    for (int c : count) {
        if (c != 0)
            return false;
    }
    
    return true;
}
```

## String Dynamic Programming

### Longest Common Subsequence

```cpp
int longestCommonSubsequence(string s1, string s2) {
    int m = s1.length();
    int n = s2.length();
    
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    
    return dp[m][n];
}
```

### Edit Distance

```cpp
int editDistance(string s1, string s2) {
    int m = s1.length();
    int n = s2.length();
    
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    
    // Base cases
    for (int i = 0; i <= m; i++)
        dp[i][0] = i;
    
    for (int j = 0; j <= n; j++)
        dp[0][j] = j;
    
    // Fill dp table
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1])
                dp[i][j] = dp[i-1][j-1];
            else
                dp[i][j] = 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
        }
    }
    
    return dp[m][n];
}
```

## Basic Data Structures

### Trie (Prefix Tree)

A tree-like data structure useful for searching words in a dictionary.

```cpp
class TrieNode {
public:
    bool isEndOfWord;
    TrieNode* children[26]; // For lowercase English letters
    
    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++)
            children[i] = nullptr;
    }
};

class Trie {
private:
    TrieNode* root;
    
public:
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* current = root;
        
        for (char c : word) {
            int index = c - 'a';
            if (!current->children[index])
                current->children[index] = new TrieNode();
            current = current->children[index];
        }
        
        current->isEndOfWord = true;
    }
    
    bool search(string word) {
        TrieNode* current = root;
        
        for (char c : word) {
            int index = c - 'a';
            if (!current->children[index])
                return false;
            current = current->children[index];
        }
        
        return current->isEndOfWord;
    }
    
    bool startsWith(string prefix) {
        TrieNode* current = root;
        
        for (char c : prefix) {
            int index = c - 'a';
            if (!current->children[index])
                return false;
            current = current->children[index];
        }
        
        return true;
    }
};
```

## Common Problems

1. **Substring Search**: Finding occurrences of a pattern in a text
2. **Palindrome Detection**: Checking if a string reads the same backward as forward
3. **Anagram Detection**: Checking if two strings contain the same characters in different order
4. **String Compression**: Encoding strings to reduce their size
5. **String Tokenization**: Breaking a string into tokens based on delimiters
6. **Wildcard Matching**: Matching patterns containing wildcards like '*' and '?'
7. **Prefix/Suffix Analysis**: Finding common prefixes or suffixes in a set of strings