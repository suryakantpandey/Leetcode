# Intuition
The problem requires finding the shortest path from the top-left corner to the bottom-right corner of a grid while minimizing the number of obstacles removed. Observing the grid, it is clear that we need to prioritize paths with fewer obstacles. This can be effectively solved using a **0-1 BFS** approach, where obstacle-free moves are given higher priority by processing them first.

# Approach
1. **Grid Representation**: 
   - Each cell in the grid represents either an empty space (`0`) or an obstacle (`1`).
   - Use a `dist` array to store the minimum number of obstacles removed to reach each cell.

2. **0-1 BFS**:
   - Use a deque to efficiently manage the exploration of grid cells:
     - Push cells to the **front** of the deque if no obstacle is encountered (`0`).
     - Push cells to the **back** of the deque if an obstacle is encountered (`1`).
   - Begin from the top-left corner `(0, 0)` and process all reachable neighbors while updating their `dist` values.

3. **Path Exploration**:
   - Move in four possible directions: up, down, left, and right.
   - For each neighbor, calculate the new obstacle count based on the current cell.
   - Update the `dist` array and push the neighbor into the deque if a better path is found.

4. **End Condition**:
   - When the bottom-right corner `(m-1, n-1)` is processed, the value in `dist[m-1][n-1]` represents the minimum obstacles removed to reach that cell.

# Complexity
- **Time Complexity**: 
  - Each cell is processed at most once, and each edge is considered once.
  - Total complexity is \(O(m \cdot n)\), where \(m\) is the number of rows and \(n\) is the number of columns in the grid.

- **Space Complexity**: 
  - The `dist` array and the deque require \(O(m \cdot n)\) space.

# Code
```cpp
class Solution {
public:
    int minimumObstacles(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();

        // Step 1: Create distance array to track obstacles removed
        vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
        dist[0][0] = 0;

        // Step 2: Initialize deque
        deque<pair<int, int>> dq;
        dq.push_front({0, 0}); // Start from the top-left corner

        // Directions for moving in the grid
        vector<pair<int, int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

        // Step 3: Process cells using 0-1 BFS
        while (!dq.empty()) {
            auto [x, y] = dq.front();
            dq.pop_front();

            for (auto [dx, dy] : directions) {
                int nx = x + dx, ny = y + dy;

                // Check bounds
                if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                    int newDist = dist[x][y] + grid[nx][ny];

                    // Update distance if a better path is found
                    if (newDist < dist[nx][ny]) {
                        dist[nx][ny] = newDist;
                        if (grid[nx][ny] == 1) {
                            dq.push_back({nx, ny}); // Push to the back if an obstacle is removed
                        } else {
                            dq.push_front({nx, ny}); // Push to the front if no obstacle is removed
                        }
                    }
                }
            }
        }

        // Return the distance to the bottom-right corner
        return dist[m - 1][n - 1];
    }
};
