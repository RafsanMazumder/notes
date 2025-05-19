# Backtracking Problems

**Subsets**

Given an integer array nums of unique elements, return all possible subsets.

```cpp
class Solution {
public:
    void backtrack(int curIndex, vector<int>& subset, vector<vector<int>>& subsets, vector<int>& nums) {
        if (curIndex == nums.size()) {
            subsets.push_back(subset);
            return;
        }

        backtrack(curIndex+1, subset, subsets, nums);

        subset.push_back(nums[curIndex]);
        backtrack(curIndex+1, subset, subsets, nums);
        subset.pop_back();
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> subset;
        vector<vector<int>> subsets;
        
        backtrack(0, subset, subsets, nums);
        return subsets;
    }
};
```

**Combination Sum**

Given an array of distinct integers and a target integer, return a list of all unique combinations where the chosen
numbers sum to target. The same number may be chosen an unlimited number of times.

```cpp

```

**Permutations**

Given an array of distinct integers, return all possible permutations.

```cpp
class Solution {
public:
    void backtrack(vector<int>& cur, vector<vector<int>>& ans, vector<bool>& visited, vector<int>& nums) {
        if (cur.size() == nums.size()) {
            ans.push_back(cur);
            return;
        }

        for (int i = 0; i < nums.size(); i++) {
            if (!visited[i]) {
                cur.push_back(nums[i]);
                visited[i] = true;
                backtrack(cur, ans, visited, nums);
                cur.pop_back();
                visited[i] = false;
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> cur;
        vector<vector<int>> ans;
        vector<bool> visited(nums.size(), false);

        backtrack(cur, ans, visited, nums);
        return ans;
    }
};
```

**Words Search**

Given an m x n grid of characters and a string word, return true if word exists in the grid.

```cpp
class Solution {
public:
    vector<int> dx = {-1, 1, 0, 0};
    vector<int> dy = {0, 0, -1, 1};

    bool backtrack(int x, int y, int wordPos, vector<vector<char>>& board, string& word) {
        if (wordPos == word.length()) return true;

        for (int i = 0; i < 4; i++) {
            int newX = x + dx[i];
            int newY = y + dy[i];

            if (newX >= 0 && newX < board.size() && newY >= 0 && newY < board[0].size() && board[newX][newY] == word[wordPos]) {
                board[newX][newY] = '#';
                if (backtrack(newX, newY, wordPos+1, board, word)) return true;
                board[newX][newY] = word[wordPos];
            }
        }
        return false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                if (board[i][j] == word[0]) {
                    board[i][j] = '#';
                    if (backtrack(i, j, 1, board, word)) return true;
                    board[i][j] = word[0];
                }
            }
        }
        return false;
    }
};
```

**N-Queens**

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.
Given an integer n, return all distinct solutions to the n-queens puzzle.

```cpp
class Solution {
public:
    vector<string> createEmptyBoard(int n) {
        string emptyRow = "";
        for (int col = 0; col < n; col++) emptyRow += '.';
        vector<string> board;
        for (int row = 0; row < n; row++) board.push_back(emptyRow);
        return board;
    }

    void backtrack(int row, vector<string> &board, vector<vector<string>>& results, unordered_set<int>& cols, unordered_set<int>& diagonals, unordered_set<int>& antiDiagonals) {
        if (row == board.size()) {
            results.push_back(board);
            return;
        }

        for (int col = 0; col < board[0].size(); col++) {
            int curDiagonal = row - col;
            int curAntiDiagonal = row + col;

            if (cols.find(col) == cols.end() && diagonals.find(curDiagonal) == diagonals.end() && antiDiagonals.find(curAntiDiagonal) == antiDiagonals.end()) {
                cols.insert(col);
                diagonals.insert(curDiagonal);
                antiDiagonals.insert(curAntiDiagonal);
                board[row][col] = 'Q';

                backtrack(row+1, board, results, cols, diagonals, antiDiagonals);

                cols.erase(col);
                diagonals.erase(curDiagonal);
                antiDiagonals.erase(curAntiDiagonal);
                board[row][col] = '.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<string> emptyBoard = createEmptyBoard(n);
        unordered_set<int> cols;
        unordered_set<int> diagonals;
        unordered_set<int> antiDiagonals;
        vector<vector<string>> results;

        backtrack(0, emptyBoard, results, cols, diagonals, antiDiagonals);
        return results;
    }
};
```