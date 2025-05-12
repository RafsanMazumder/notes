# Graph Algorithms

A comprehensive guide to common graph algorithms with implementation details and use cases.

## Graph Representations

### Adjacency Matrix

```cpp
vector<vector<int>> graph(n, vector<int>(n, 0));
// Add edge from u to v
graph[u][v] = 1;  // For weighted graph: graph[u][v] = weight
```

**Pros:**
- O(1) lookup time to check if edge exists
- Simple implementation for dense graphs

**Cons:**
- O(V²) space complexity
- Inefficient for sparse graphs

### Adjacency List

```cpp
vector<vector<int>> graph(n);
// Add edge from u to v
graph[u].push_back(v);

// For weighted graph:
vector<vector<pair<int, int>>> graph(n);
// Add edge from u to v with weight w
graph[u].push_back({v, w});
```

**Pros:**
- Space efficient for sparse graphs: O(V+E)
- Faster to iterate over edges

**Cons:**
- O(degree(v)) time to check if edge exists
- Less intuitive for dense graphs

## Graph Traversal

### Depth-First Search (DFS)

```cpp
void dfs(vector<vector<int>>& graph, int node, vector<bool>& visited) {
    visited[node] = true;
    cout << node << " ";
    
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfs(graph, neighbor, visited);
        }
    }
}

void dfsTraversal(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);
    dfs(graph, start, visited);
}
```

**Time Complexity:** O(V + E)  
**Space Complexity:** O(V) for the recursion stack

### Breadth-First Search (BFS)

```cpp
void bfs(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);
    queue<int> q;
    
    visited[start] = true;
    q.push(start);
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        cout << node << " ";
        
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

**Time Complexity:** O(V + E)  
**Space Complexity:** O(V) for the queue

## Shortest Path Algorithms

### Dijkstra's Algorithm

For finding shortest paths from a source to all vertices in a weighted graph with non-negative weights.

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
        
        for (auto& edge : graph[u]) {
            int v = edge.first;
            int weight = edge.second;
            
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    
    return dist;
}
```

**Time Complexity:** O(E log V)

### Bellman-Ford Algorithm

For finding shortest paths from a source to all vertices, even with negative edge weights (but no negative cycles).

```cpp
vector<int> bellmanFord(int n, vector<vector<int>>& edges, int start) {
    vector<int> dist(n, INT_MAX);
    dist[start] = 0;
    
    // Relax all edges V-1 times
    for (int i = 0; i < n - 1; i++) {
        for (auto& edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int weight = edge[2];
            
            if (dist[u] != INT_MAX && dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
            }
        }
    }
    
    // Check for negative cycles
    for (auto& edge : edges) {
        int u = edge[0];
        int v = edge[1];
        int weight = edge[2];
        
        if (dist[u] != INT_MAX && dist[u] + weight < dist[v]) {
            cout << "Graph contains negative weight cycle" << endl;
            return {};
        }
    }
    
    return dist;
}
```

**Time Complexity:** O(V * E)

### Floyd-Warshall Algorithm

For finding shortest paths between all pairs of vertices.

```cpp
vector<vector<int>> floydWarshall(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<vector<int>> dist = graph;
    
    // Initialize the distance matrix
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i != j && dist[i][j] == 0) {
                dist[i][j] = INT_MAX;
            }
        }
    }
    
    // Update the shortest path for all pairs of vertices
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (dist[i][k] != INT_MAX && dist[k][j] != INT_MAX && 
                    dist[i][k] + dist[k][j] < dist[i][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
    
    return dist;
}
```

**Time Complexity:** O(V³)

## Minimum Spanning Tree

### Kruskal's Algorithm

```cpp
int kruskalMST(int n, vector<vector<int>>& edges) {
    // Sort edges by weight
    sort(edges.begin(), edges.end(), [](const vector<int>& a, const vector<int>& b) {
        return a[2] < b[2];
    });
    
    // Initialize disjoint set
    vector<int> parent(n);
    vector<int> rank(n, 0);
    for (int i = 0; i < n; i++) {
        parent[i] = i;
    }
    
    function<int(int)> find = [&](int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    };
    
    auto unionSets = [&](int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return false;
        
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        
        return true;
    };
    
    int totalWeight = 0;
    int edgesAdded = 0;
    
    for (auto& edge : edges) {
        int u = edge[0];
        int v = edge[1];
        int weight = edge[2];
        
        if (unionSets(u, v)) {
            totalWeight += weight;
            edgesAdded++;
            
            if (edgesAdded == n - 1) break;
        }
    }
    
    return totalWeight;
}
```

**Time Complexity:** O(E log E)

### Prim's Algorithm

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
        
        for (auto& neighbor : graph[u]) {
            int v = neighbor.first;
            int w = neighbor.second;
            
            if (!inMST[v] && w < key[v]) {
                key[v] = w;
                pq.push({key[v], v});
            }
        }
    }
    
    return totalWeight;
}
```

**Time Complexity:** O(E log V)

## Strongly Connected Components

### Kosaraju's Algorithm

```cpp
void dfs(vector<vector<int>>& graph, int node, vector<bool>& visited, vector<int>& order) {
    visited[node] = true;
    
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfs(graph, neighbor, visited, order);
        }
    }
    
    order.push_back(node);
}

void dfsReverse(vector<vector<int>>& reversedGraph, int node, vector<bool>& visited, vector<int>& component) {
    visited[node] = true;
    component.push_back(node);
    
    for (int neighbor : reversedGraph[node]) {
        if (!visited[neighbor]) {
            dfsReverse(reversedGraph, neighbor, visited, component);
        }
    }
}

vector<vector<int>> kosarajuSCC(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<bool> visited(n, false);
    vector<int> order;
    
    // Step 1: DFS and record finish order
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(graph, i, visited, order);
        }
    }
    
    // Step 2: Reverse the graph
    vector<vector<int>> reversedGraph(n);
    for (int u = 0; u < n; u++) {
        for (int v : graph[u]) {
            reversedGraph[v].push_back(u);
        }
    }
    
    // Step 3: DFS on reversed graph in finish order
    fill(visited.begin(), visited.end(), false);
    vector<vector<int>> scc;
    
    for (int i = n - 1; i >= 0; i--) {
        int node = order[i];
        if (!visited[node]) {
            vector<int> component;
            dfsReverse(reversedGraph, node, visited, component);
            scc.push_back(component);
        }
    }
    
    return scc;
}
```

**Time Complexity:** O(V + E)
**Space Complexity:** O(V) for the recursion stack and visited array
