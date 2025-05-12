# Dynamic Programming

Dynamic Programming (DP) is an algorithmic technique for solving complex problems by breaking them down into simpler subproblems and storing the results to avoid redundant calculations.

## Key Principles

1. **Optimal Substructure**: An optimal solution contains optimal solutions to its subproblems
2. **Overlapping Subproblems**: Same subproblems are solved multiple times

## Implementation Approaches

### 1. Top-Down (Memoization)

```cpp
// Fibonacci with memoization
int fib(int n, vector<int>& memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    
    memo[n] = fib(n-1, memo) + fib(n-2, memo);
    return memo[n];
}

int fibonacci(int n) {
    vector<int> memo(n+1, -1);
    return fib(n, memo);
}
```

### 2. Bottom-Up (Tabulation)

```cpp
// Fibonacci with tabulation
int fibonacci(int n) {
    if (n <= 1) return n;
    
    vector<int> dp(n+1);
    dp[0] = 0;
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    
    return dp[n];
}
```

## Classic DP Problems

### 1. Knapsack Problem

```cpp
// 0-1 Knapsack
int knapsack(vector<int>& values, vector<int>& weights, int capacity) {
    int n = values.size();
    vector<vector<int>> dp(n+1, vector<int>(capacity+1, 0));
    
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= capacity; w++) {
            if (weights[i-1] <= w) {
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w - weights[i-1]],  // Take item
                    dp[i-1][w]                                // Don't take item
                );
            } else {
                dp[i][w] = dp[i-1][w];  // Can't take item
            }
        }
    }
    
    return dp[n][capacity];
}
```

### 2. Longest Common Subsequence

```cpp
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.length(), n = text2.length();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1[i-1] == text2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    
    return dp[m][n];
}
```

### 3. Coin Change

```cpp
int coinChange(vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, amount + 1);
    dp[0] = 0;
    
    for (int coin : coins) {
        for (int i = coin; i <= amount; i++) {
            dp[i] = min(dp[i], dp[i - coin] + 1);
        }
    }
    
    return dp[amount] > amount ? -1 : dp[amount];
}
```

### 4. Longest Increasing Subsequence

```cpp
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, 1);
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
    }
    
    return *max_element(dp.begin(), dp.end());
}
```

### 5. Edit Distance

```cpp
int minDistance(string word1, string word2) {
    int m = word1.length(), n = word2.length();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
    
    // Base cases
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1[i-1] == word2[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = 1 + min({
                    dp[i-1][j],    // Delete
                    dp[i][j-1],    // Insert
                    dp[i-1][j-1]   // Replace
                });
            }
        }
    }
    
    return dp[m][n];
}
```

## Optimization Techniques

### 1. Space Optimization

Often, DP solutions can be optimized from O(n²) to O(n) space by only storing the most recent rows/columns:

```cpp
// Fibonacci with O(1) space
int fibonacci(int n) {
    if (n <= 1) return n;
    
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    
    return curr;
}
```

### 2. State Compression

For problems with boolean states, bit manipulation can optimize space:

```cpp
// Using bitmasks in DP
int countWays(int n, int mask, vector<int>& dp) {
    if (mask == (1 << n) - 1) return 1;
    if (dp[mask] != -1) return dp[mask];
    
    int ways = 0;
    for (int i = 0; i < n; i++) {
        if ((mask & (1 << i)) == 0) {
            ways += countWays(n, mask | (1 << i), dp);
        }
    }
    
    return dp[mask] = ways;
}
```

## How to Approach DP Problems

1. **Identify** if a problem can be solved with DP (optimal substructure, overlapping subproblems)
2. **Define state** clearly (what does each dp[i] or dp[i][j] represent?)
3. **Establish recurrence relation** (how to build solutions from smaller subproblems)
4. **Determine base cases**
5. **Decide** between top-down or bottom-up implementation
6. **Optimize** for space if needed