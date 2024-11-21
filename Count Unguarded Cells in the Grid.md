# [2257. Count Unguarded Cells in the Grid](https://leetcode.com/problems/count-unguarded-cells-in-the-grid/)

### Problem Statement:
You are given two integers `m` and `n` representing a 0-indexed `m x n` grid. You are also given two 2D integer arrays `guards` and `walls` where `guards[i] = [rowi, coli]` and `walls[j] = [rowj, colj]` represent the positions of the `ith` guard and `jth` wall respectively.

A guard can see every cell in the four cardinal directions (north, east, south, or west) starting from their position unless obstructed by a wall or another guard. A cell is guarded if there is at least one guard that can see it.

Return the number of unoccupied cells that are not guarded.

---

### Examples:

#### Example 1:
**Input:**  
`m = 4, n = 6, guards = [[0,0],[1,1],[2,3]], walls = [[0,1],[2,2],[1,4]]`  
**Output:**  
`7`  
**Explanation:**  
The guarded and unguarded cells are shown in red and green respectively in the diagram.  
There are a total of 7 unguarded cells.

#### Example 2:
**Input:**  
`m = 3, n = 3, guards = [[1,1]], walls = [[0,1],[1,0],[2,1],[1,2]]`  
**Output:**  
`4`  
**Explanation:**  
The unguarded cells are shown in green in the diagram.  
There are a total of 4 unguarded cells.

---

### Constraints:
- 1 ≤ m, n ≤ 10^5
- 2 ≤ m * n ≤ 10^5
- 1 ≤ guards.length, walls.length ≤ 5 * 10^4
- 2 ≤ guards.length + walls.length ≤ m * n
- guards[i].length == walls[j].length == 2
- 0 ≤ rowi, rowj < m
- 0 ≤ coli, colj < n
- All the positions in `guards` and `walls` are unique.

---

### Approach:

To solve this problem, we will simulate the grid and mark cells as guarded or unguarded using a **directional propagation** strategy:

1. **Grid Representation:**
   - Create a grid where:
     - `0` represents unguarded cells.
     - `1` represents guarded cells.
     - `2` represents guards.
     - `3` represents walls.

2. **Mark Guards and Walls:**
   - Place guards and walls on the grid using their positions.

3. **Directional Propagation:**
   - For each guard, mark all the cells in the four cardinal directions as guarded until a wall or another guard is encountered.

4. **Count Unguarded Cells:**
   - Traverse the grid and count the cells that remain unguarded (`0`).

---

### Code Implementation:

```cpp
class Solution {
public:
    int countUnguarded(int m, int n, vector<vector<int>>& guards, vector<vector<int>>& walls) {
        vector<vector<int>> grid(m, vector<int>(n, 0));

        // Mark guards and walls on the grid
        for (auto& g : guards) grid[g[0]][g[1]] = 2; // Guard
        for (auto& w : walls) grid[w[0]][w[1]] = 3; // Wall

        // Directions for cardinal points: [down, up, right, left]
        vector<pair<int, int>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        // Function to mark guarded cells
        auto markGuarded = [&](int x, int y, int dx, int dy) {
            while (x >= 0 && x < m && y >= 0 && y < n) {
                if (grid[x][y] == 2 || grid[x][y] == 3) break; // Stop at a guard or a wall
                if (grid[x][y] == 0) grid[x][y] = 1; // Mark as guarded
                x += dx, y += dy;
            }
        };

        // Mark all cells guarded by each guard
        for (auto& g : guards) {
            for (auto& dir : directions) {
                markGuarded(g[0] + dir.first, g[1] + dir.second, dir.first, dir.second);
            }
        }

        // Count unguarded cells
        int unguardedCount = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) ++unguardedCount;
            }
        }

        return unguardedCount;
    }
};
