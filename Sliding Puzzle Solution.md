
# Sliding Puzzle Solution

## Intuition
The problem is essentially finding the shortest sequence of moves to solve a sliding puzzle. This can be visualized as a graph traversal problem, where each board state is a node and valid moves are edges connecting these nodes. The shortest path can be efficiently found using Breadth-First Search (BFS), as it explores all states level by level.

## Approach
1. **String Representation**: Convert the 2x3 board into a single string to simplify comparisons and manipulations. The goal is to transform the given string into the target string `"123450"`.
2. **Adjacency List**: Predefine which indices in the string can swap with the `0` for a valid move. This represents the connectivity of the graph based on the board's layout.
3. **Breadth-First Search (BFS)**:
   - Start with the initial board configuration.
   - Use a queue to store the current state and the number of moves taken to reach it.
   - Keep a `visited` set to avoid revisiting the same configuration.
   - For each state, find all possible next states by swapping `0` with its neighbors. If the target state is reached, return the move count.
4. **Edge Cases**: If BFS completes without finding the target state, return `-1`.

## Complexity
- **Time complexity**:  
  The total number of unique states in the 2x3 sliding puzzle is at most \\(6!\\) (720) because there are 6 tiles that can be arranged in any order. Each state can have up to 4 neighbors (swaps).  
  Therefore, the complexity is bounded by \\(O(V + E) = O(720 + 4 \\times 720) = O(720)\\), which is constant.
  
- **Space complexity**:  
  The space required for the queue and the visited set is \\(O(720)\\), which is also constant.

## Code
```cpp
class Solution {
public:
    int slidingPuzzle(vector<vector<int>>& board) {
        // Convert the 2x3 board to a string representation.
        string start = "";
        for (auto& row : board) {
            for (int num : row) {
                start += to_string(num);
            }
        }

        string target = "123450"; // The solved board state.

        // Adjacency list for the 2x3 board's zero position.
        vector<vector<int>> adj = {
            {1, 3},       // 0 can swap with 1, 3
            {0, 2, 4},    // 1 can swap with 0, 2, 4
            {1, 5},       // 2 can swap with 1, 5
            {0, 4},       // 3 can swap with 0, 4
            {1, 3, 5},    // 4 can swap with 1, 3, 5
            {2, 4}        // 5 can swap with 2, 4
        };

        // BFS setup
        queue<pair<string, int>> q; // {current board, number of moves}
        unordered_set<string> visited; // Keep track of visited states

        q.push({start, 0});
        visited.insert(start);

        while (!q.empty()) {
            auto [current, moves] = q.front();
            q.pop();

            // If we reach the target state, return the number of moves.
            if (current == target) {
                return moves;
            }

            // Find the position of '0' in the current board string.
            int zeroPos = current.find('0');

            // Explore all possible moves for the '0' position.
            for (int neighbor : adj[zeroPos]) {
                string next = current;
                swap(next[zeroPos], next[neighbor]); // Swap '0' with its neighbor.

                // If this state is not visited, enqueue it.
                if (visited.find(next) == visited.end()) {
                    visited.insert(next);
                    q.push({next, moves + 1});
                }
            }
        }

        // If BFS completes without finding the target, return -1.
        return -1;
    }
};
