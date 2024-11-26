# Find the Champion Node in a Directed Graph

## Intuition
The problem is about finding a single node in a directed graph with specific properties. The key observation here is:
- A node with `in-degree = 0` has no incoming edges, making it a candidate for being the "champion node."
- If there is more than one such node, or none at all, the graph does not satisfy the requirements for a single champion node.

The goal is to count the nodes with `in-degree = 0` and return it if there is exactly one.

---

## Approach
1. **Calculate In-Degree**:
   - For every node in the graph, count the number of incoming edges. This can be efficiently calculated using an array `inDegree` of size \(n\), where \(n\) is the number of nodes.

2. **Identify Nodes with `in-degree = 0`**:
   - Traverse through the `inDegree` array and count how many nodes have `in-degree = 0`.
   - Keep track of the node with `in-degree = 0` (if one exists).

3. **Check for a Unique Node**:
   - If there is exactly one node with `in-degree = 0`, return that node.
   - Otherwise, return `-1`.

---

## Complexity
- **Time Complexity**: 
  - Calculating in-degree: \(O(E)\), where \(E\) is the number of edges.
  - Iterating through nodes to check in-degree: \(O(V)\), where \(V\) is the number of nodes.
  - **Total**: \(O(V + E)\).

- **Space Complexity**:
  - The `inDegree` array requires \(O(V)\) space.
  - **Total**: \(O(V)\).

---

## Code
```cpp
class Solution {
public:
   int findChampion(int n, vector<vector<int>>& edges) {
        // Step 1: Calculate in-degree for all nodes
        vector<int> inDegree(n, 0);
        for (auto& edge : edges) {
            inDegree[edge[1]]++; // Increment in-degree for destination node
        }

        // Step 2: Identify nodes with in-degree = 0
        int node = -1;
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            if (inDegree[i] == 0) {
                cnt++;
                node = i;
            }
        }

        // Step 3: Check if there is exactly one node with in-degree = 0
        if (cnt == 1) {
            return node;
        }

        // If there are no such nodes or multiple nodes with in-degree = 0
        return -1;
    }
};
