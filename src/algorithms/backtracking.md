# Backtracking Algorithms

Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, and removing those solutions that fail to satisfy the constraints of the problem.

## Core Concept

The backtracking algorithm traverses the search tree recursively, from the root down. As it traverses, it:

1. Chooses one option from the available choices
2. Explores that option recursively
3. If that option leads to a solution, great!
4. If not, **backtracks** to explore other options

## Template for Backtracking Problems

```cpp
void backtrack(state, choices) {
    // Base case: if we've found a solution
    if (isValidSolution(state)) {
        storeSolution(state);
        return;
    }
    
    // Try each available choice
    for (choice in availableChoices) {
        // Skip invalid choices early
        if (!isValid(state, choice)) continue;
        
        // Make the choice
        applyChoice(state, choice);
        
        // Recurse
        backtrack(state, remainingChoices);
        
        // Undo the choice (backtrack)
        undoChoice(state, choice);
    }
}
```

## Common Backtracking Problems

### N-Queens

```cpp
bool isValid(vector<int>& queens, int row, int col) {
    for (int i = 0; i < row; i++) {
        // Check vertical and diagonal attacks
        if (queens[i] == col || abs(queens[i] - col) == abs(i - row))
            return false;
    }
    return true;
}

void solveNQueens(int n, int row, vector<int>& queens, vector<vector<string>>& results) {
    if (row == n) {
        // Found a valid placement of queens
        storeSolution(queens, results);
        return;
    }
    
    for (int col = 0; col < n; col++) {
        if (isValid(queens, row, col)) {
            queens[row] = col;
            solveNQueens(n, row + 1, queens, results);
            // No explicit backtracking needed, as we overwrite queens[row]
        }
    }
}
```

### Permutations

```cpp
void generatePermutations(vector<int>& nums, int start, vector<vector<int>>& result) {
    if (start == nums.size()) {
        result.push_back(nums);
        return;
    }
    
    for (int i = start; i < nums.size(); i++) {
        // Swap to create a new permutation
        swap(nums[start], nums[i]);
        
        // Recurse
        generatePermutations(nums, start + 1, result);
        
        // Backtrack - restore the original array
        swap(nums[start], nums[i]);
    }
}
```

## Time Complexity

Most backtracking algorithms have exponential time complexity:
- **N-Queens**: O(N!)
- **Permutations**: O(N!)
- **Combinations**: O(2^N)

## When to Use Backtracking

- Problems where you need to find all (or some) solutions
- Constraint satisfaction problems
- Combinatorial optimization
- Problems where you need to explore all possibilities