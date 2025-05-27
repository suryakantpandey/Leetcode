# [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

### Problem:
Given an `m x n` integers matrix, return the length of the **longest increasing path** in the matrix.

From each cell, you can either move in four directions: left, right, up, or down.  
You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).

---

### Example 1:

**Input:** `matrix = [[9,9,4],[6,6,8],[2,1,1]]`  
**Output:** `4`  
**Explanation:** The longest increasing path is `[1, 2, 6, 9]`.

### Example 2:

**Input:** `matrix = [[3,4,5],[3,2,6],[2,2,1]]`  
**Output:** `4`  
**Explanation:** The longest increasing path is `[3, 4, 5, 6]`.

### Example 3:

**Input:** `matrix = [[1]]`  
**Output:** `1`

---

### Constraints:
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `0 <= matrix[i][j] <= 2^31 - 1`

---

## Approach:

To solve this problem, we can use **DFS with memoization**:

1. For each cell, try moving in all four directions (up, down, left, right).
2. Only move to a cell if it has a strictly greater value.
3. Use memoization to store the result of longest increasing path starting at each cell to avoid recomputation.
4. Return the maximum path length found from any cell.

Time complexity: **O(m * n)**  
Space complexity: **O(m * n)**

---

## Code:
```cpp
class Solution {
public:
    const vector<pair<int, int>> directions = {
        {-1, 0}, // UP
        {1, 0},  // DOWN
        {0, -1}, // LEFT
        {0, 1}   // RIGHT
    };

    int rows, cols;
    vector<vector<int>> grid;
    vector<vector<int>> memo;

    int dfs(int row, int col) {
        if (memo[row][col] != -1) return memo[row][col];

        int maxPathLength = 1;

        for (auto [dx, dy] : directions) {
            int newRow = row + dx;
            int newCol = col + dy;

            if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols &&
                grid[newRow][newCol] > grid[row][col]) {

                maxPathLength = max(maxPathLength, 1 + dfs(newRow, newCol));
            }
        }

        return memo[row][col] = maxPathLength;
    }

    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return 0;

        grid = matrix;
        rows = matrix.size();
        cols = matrix[0].size();
        memo.assign(rows, vector<int>(cols, -1));

        int longestPath = 0;
        for (int row = 0; row < rows; ++row) {
            for (int col = 0; col < cols; ++col) {
                longestPath = max(longestPath, dfs(row, col));
            }
        }

        return longestPath;
    }
};
```
