# Data Structures

A comprehensive guide to common data structures with implementation details and time complexity analysis.

## Arrays

Arrays store elements in contiguous memory locations, providing O(1) access by index.

```cpp
// C++ array declaration
int staticArray[5] = {1, 2, 3, 4, 5};

// Dynamic array (vector in C++)
vector<int> dynamicArray = {1, 2, 3, 4, 5};
```

**Time Complexities:**
- Access: O(1)
- Search: O(n)
- Insert/Delete at end: O(1) amortized
- Insert/Delete at arbitrary position: O(n)

## Linked Lists

Linked lists store elements in nodes, each pointing to the next node in the sequence.

```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```

**Time Complexities:**
- Access: O(n)
- Search: O(n)
- Insert/Delete at beginning: O(1)
- Insert/Delete at end: O(n) (O(1) with tail pointer)
- Insert/Delete at position: O(n)

## Stacks

LIFO (Last In, First Out) data structure.

```cpp
stack<int> s;
s.push(1);
s.push(2);
int top = s.top(); // 2
s.pop();
```

**Time Complexities:**
- Push: O(1)
- Pop: O(1)
- Top: O(1)

## Queues

FIFO (First In, First Out) data structure.

```cpp
queue<int> q;
q.push(1);
q.push(2);
int front = q.front(); // 1
q.pop();
```

**Time Complexities:**
- Push: O(1)
- Pop: O(1)
- Front: O(1)

## Hash Tables

Hash tables map keys to values using a hash function.

```cpp
unordered_map<string, int> hashMap;
hashMap["apple"] = 5;
int value = hashMap["apple"]; // 5
```

**Time Complexities:**
- Insert: O(1) average, O(n) worst
- Delete: O(1) average, O(n) worst
- Search: O(1) average, O(n) worst

## Trees

### Binary Tree

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

### Binary Search Tree (BST)

```cpp
// BST Property: left->val < node->val < right->val

// Search in BST
TreeNode* search(TreeNode* root, int key) {
    if (!root || root->val == key) return root;
    if (key < root->val) return search(root->left, key);
    return search(root->right, key);
}
```

**Time Complexities (Balanced BST):**
- Search: O(log n)
- Insert: O(log n)
- Delete: O(log n)

## Heaps

A binary heap is a complete binary tree where each node is either greater than or equal to (max-heap) or less than or equal to (min-heap) its children.

```cpp
// Min-heap in C++
priority_queue<int, vector<int>, greater<int>> minHeap;
minHeap.push(3);
minHeap.push(1);
minHeap.push(2);
int min = minHeap.top(); // 1
```

**Time Complexities:**
- Insert: O(log n)
- Extract Min/Max: O(log n)
- Peek: O(1)
- Heapify: O(n)

## Graphs

Graphs consist of nodes (vertices) and edges connecting them.

**Representation:**

1. Adjacency Matrix:
```cpp
vector<vector<int>> graph(n, vector<int>(n, 0));
// Add edge from u to v
graph[u][v] = 1;
```

2. Adjacency List:
```cpp
vector<vector<int>> graph(n);
// Add edge from u to v
graph[u].push_back(v);
```

**Time Complexities (Adjacency List):**
- Add Edge: O(1)
- Remove Edge: O(E) where E is the number of edges
- Check if edge exists: O(E)

## Trie

A tree-like data structure used for storing a dynamic set of strings.

```cpp
struct TrieNode {
    TrieNode* children[26];
    bool isEndOfWord;
    
    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++)
            children[i] = nullptr;
    }
};
```

**Time Complexities:**
- Insert: O(L) where L is the length of the string
- Search: O(L)
- Delete: O(L)

## Union-Find (Disjoint Set)

Used to track disjoint sets and perform union operations on them.

```cpp
class DisjointSet {
    vector<int> parent, rank;
public:
    DisjointSet(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for (int i = 0; i < n; i++)
            parent[i] = i;
    }
    
    int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]);
        return parent[x];
    }
    
    void unionSets(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return;
        
        if (rank[rootX] < rank[rootY])
            parent[rootX] = rootY;
        else if (rank[rootX] > rank[rootY])
            parent[rootY] = rootX;
        else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
    }
};
```

**Time Complexities (with path compression and union by rank):**
- Find: O(α(n)) (effectively O(1))
- Union: O(α(n)) (effectively O(1))