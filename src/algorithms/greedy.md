# Greedy Algorithms

Greedy algorithms make locally optimal choices at each step with the hope of finding a global optimum. They are typically used for optimization problems.

## Characteristics of Greedy Algorithms

1. **Greedy Choice Property**: A global optimum can be reached by making locally optimal choices.
2. **Optimal Substructure**: An optimal solution contains optimal solutions to its subproblems.
3. **Simple Implementation**: Usually easier to implement compared to other techniques.
4. **No Backtracking**: Once a decision is made, it is never reconsidered.

## Common Greedy Problems

### 1. Coin Change (with specific coin systems)

For coin systems like US currency (1, 5, 10, 25), a greedy approach works.

```cpp
vector<int> greedyCoinChange(int amount, vector<int>& coins) {
    // Sort coins in descending order
    sort(coins.rbegin(), coins.rend());
    
    vector<int> result;
    
    for (int coin : coins) {
        while (amount >= coin) {
            result.push_back(coin);
            amount -= coin;
        }
    }
    
    return result;
}
```

**Note**: This doesn't work for all coin systems. For example, with coins {1, 3, 4} and amount 6, the optimal solution is two 3's, not a 4 and two 1's.

### 2. Activity Selection

Select the maximum number of activities that don't overlap.

```cpp
vector<int> activitySelection(vector<pair<int, int>>& activities) {
    // Sort by end time
    sort(activities.begin(), activities.end(), 
         [](pair<int, int>& a, pair<int, int>& b) {
             return a.second < b.second;
         });
    
    vector<int> selected;
    selected.push_back(0);  // First activity is always selected
    
    int lastEnd = activities[0].second;
    
    for (int i = 1; i < activities.size(); i++) {
        // If this activity starts after the last selected activity ends
        if (activities[i].first >= lastEnd) {
            selected.push_back(i);
            lastEnd = activities[i].second;
        }
    }
    
    return selected;
}
```

### 3. Fractional Knapsack

Unlike 0/1 Knapsack, we can take fractions of items.

```cpp
double fractionalKnapsack(vector<int>& values, vector<int>& weights, int capacity) {
    int n = values.size();
    vector<pair<double, int>> valuePerWeight(n);
    
    for (int i = 0; i < n; i++) {
        valuePerWeight[i] = {(double)values[i] / weights[i], i};
    }
    
    // Sort by value per weight in descending order
    sort(valuePerWeight.rbegin(), valuePerWeight.rend());
    
    double totalValue = 0.0;
    
    for (auto& [ratio, index] : valuePerWeight) {
        if (capacity >= weights[index]) {
            // Take the whole item
            totalValue += values[index];
            capacity -= weights[index];
        } else {
            // Take a fraction of the item
            totalValue += values[index] * ((double)capacity / weights[index]);
            break;
        }
    }
    
    return totalValue;
}
```

### 4. Huffman Coding

A lossless data compression algorithm.

```cpp
struct Node {
    char data;
    int freq;
    Node *left, *right;
    
    Node(char data, int freq) : data(data), freq(freq), left(nullptr), right(nullptr) {}
};

struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq;
    }
};

unordered_map<char, string> huffmanCodes;

void generateCodes(Node* root, string code) {
    if (!root) return;
    
    if (root->data != '\0') {
        huffmanCodes[root->data] = code;
    }
    
    generateCodes(root->left, code + "0");
    generateCodes(root->right, code + "1");
}

void huffmanCoding(string text) {
    unordered_map<char, int> freq;
    for (char c : text) {
        freq[c]++;
    }
    
    priority_queue<Node*, vector<Node*>, Compare> pq;
    
    for (auto& pair : freq) {
        pq.push(new Node(pair.first, pair.second));
    }
    
    while (pq.size() > 1) {
        Node* left = pq.top(); pq.pop();
        Node* right = pq.top(); pq.pop();
        
        Node* parent = new Node('\0', left->freq + right->freq);
        parent->left = left;
        parent->right = right;
        
        pq.push(parent);
    }
    
    Node* root = pq.top();
    generateCodes(root, "");
    
    cout << "Huffman Codes:" << endl;
    for (auto& pair : huffmanCodes) {
        cout << pair.first << ": " << pair.second << endl;
    }
}
```

### 5. Minimum Spanning Tree (Prim's Algorithm)

```cpp
int primMST(vector<vector<pair<int, int>>>& graph) {
    int n = graph.size();
    vector<bool> inMST(n, false);
    vector<int> key(n, INT_MAX);
    
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    key[0] = 0;
    pq.push({0, 0});  // {weight, vertex}
    
    int totalWeight = 0;
    
    while (!pq.empty()) {
        int u = pq.top().second;
        int weight = pq.top().first;
        pq.pop();
        
        if (inMST[u]) continue;
        
        inMST[u] = true;
        totalWeight += weight;
        
        for (auto& [v, w] : graph[u]) {
            if (!inMST[v] && w < key[v]) {
                key[v] = w;
                pq.push({key[v], v});
            }
        }
    }
    
    return totalWeight;
}
```

### 6. Dijkstra's Algorithm (Shortest Path)

```cpp
vector<int> dijkstra(vector<vector<pair<int, int>>>& graph, int start) {
    int n = graph.size();
    vector<int> dist(n, INT_MAX);
    dist[start] = 0;
    
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    pq.push({0, start});
    
    while (!pq.empty()) {
        int u = pq.top().second;
        int d = pq.top().first;
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto& [v, weight] : graph[u]) {
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    
    return dist;
}
```

### 7. Job Sequencing with Deadlines

```cpp
struct Job {
    int id, deadline, profit;
};

vector<int> jobSequencing(vector<Job>& jobs) {
    // Sort jobs by profit in descending order
    sort(jobs.begin(), jobs.end(), [](Job& a, Job& b) {
        return a.profit > b.profit;
    });
    
    // Find the maximum deadline
    int maxDeadline = 0;
    for (Job& job : jobs) {
        maxDeadline = max(maxDeadline, job.deadline);
    }
    
    vector<int> slot(maxDeadline + 1, -1);
    vector<int> sequence;
    int totalProfit = 0;
    
    for (Job& job : jobs) {
        // Find a free slot before the deadline
        for (int i = job.deadline; i > 0; i--) {
            if (slot[i] == -1) {
                slot[i] = job.id;
                totalProfit += job.profit;
                sequence.push_back(job.id);
                break;
            }
        }
    }
    
    cout << "Total profit: " << totalProfit << endl;
    return sequence;
}
```

## When to Use Greedy Algorithms

Greedy algorithms are appropriate when:

1. **The problem has optimal substructure**
2. **The greedy choice is consistent with global optimal**
3. **Local optimizations lead to global optimization**

## Advantages of Greedy Algorithms

1. **Simple and intuitive**
2. **Often efficient (typically O(n log n) due to sorting)**
3. **Doesn't require looking at all possibilities**

## Disadvantages of Greedy Algorithms

1. **Doesn't always yield the optimal solution**
2. **Difficult to prove correctness (requires mathematical proof)**
3. **Short-sighted approach might miss better solutions**

## Greedy vs. Dynamic Programming

| Greedy                              | Dynamic Programming                      |
|-------------------------------------|------------------------------------------|
| Makes locally optimal choices       | Considers all possible choices           |
| No guarantee of global optimum      | Always finds the global optimum          |
| Typically faster                    | Typically slower but more thorough       |
| No overlapping subproblems needed   | Relies on overlapping subproblems        |
| No memorization                     | Uses memoization or tabulation           |
| Examples: Fractional Knapsack       | Examples: 0/1 Knapsack                   |

## When Greedy Fails: Counterexamples

1. **Coin Change**: For denominations {1, 3, 4} and amount 6, greedy gives {4, 1, 1} (3 coins), but optimal is {3, 3} (2 coins).
2. **0/1 Knapsack**: Choosing items by value/weight ratio doesn't always yield optimal solution.

## Real-World Applications

1. **Network routing protocols**
2. **Data compression (Huffman coding)**
3. **Task scheduling**
4. **Resource allocation**
5. **Load balancing in distributed systems**