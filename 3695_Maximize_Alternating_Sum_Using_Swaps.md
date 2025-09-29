# 3695. Maximize Alternating Sum Using Swaps

**Difficulty:** Hard  

---

## Problem Statement

You are given an integer array `nums`.

You want to maximize the alternating sum of `nums`, which is defined as the value obtained by adding elements at even indices and subtracting elements at odd indices.  
That is:  

```
nums[0] - nums[1] + nums[2] - nums[3] ...
```

You are also given a 2D integer array `swaps` where `swaps[i] = [pi, qi]`. For each pair `[pi, qi]` in swaps, you are allowed to swap the elements at indices `pi` and `qi`. These swaps can be performed **any number of times** and in any order.

Return the maximum possible alternating sum of `nums`.

---

## Examples

**Example 1:**  
```
Input: nums = [1,2,3], swaps = [[0,2],[1,2]]
Output: 4

Explanation:  
The maximum alternating sum is achieved when nums is [2, 1, 3] or [3, 1, 2].  
As an example, you can obtain nums = [2, 1, 3] as follows:
- Swap nums[0] and nums[2]. nums = [3, 2, 1]
- Swap nums[1] and nums[2]. nums = [3, 1, 2]
- Swap nums[0] and nums[2]. nums = [2, 1, 3]
```

**Example 2:**  
```
Input: nums = [1,2,3], swaps = [[1,2]]
Output: 2

Explanation:  
The maximum alternating sum is achieved by not performing any swaps.
```

**Example 3:**  
```
Input: nums = [1,1000000000,1,1000000000,1,1000000000], swaps = []
Output: -2999999997

Explanation:  
Since we cannot perform any swaps, the maximum alternating sum is achieved by not performing any swaps.
```

---

## Constraints

- `2 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^9`
- `0 <= swaps.length <= 10^5`
- `swaps[i] = [pi, qi]`
- `0 <= pi < qi <= nums.length - 1`
- `[pi, qi] != [pj, qj]`

---

## Approach

1. **DSU (Disjoint Set Union):**  
   - Use DSU to group indices that can be swapped with each other (directly or indirectly).  
   - Each connected component represents indices whose values can be freely rearranged.

2. **Greedy Placement:**  
   - For each connected component, gather all values and all indices.  
   - Sort values in descending order.  
   - Assign the largest values to even indices (positive contribution) and the smaller values to odd indices (negative contribution).

3. **Final Calculation:**  
   - After rearranging all components, compute the alternating sum.

---

## Code (C++)

```cpp
struct DSU {
    vector<int> parent;
    vector<int> rank;
    
    DSU(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    void unite(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return;
        
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
    }
};

class Solution {
public:
     long long maxAlternatingSum(vector<int>& nums, vector<vector<int>>& swaps) {
        int n = nums.size();
        DSU dsu(n);
        
        // Build DSU from swaps
        for (auto& swap : swaps) {
            dsu.unite(swap[0], swap[1]);
        }
        
        // Group indices by component
        map<int, vector<int>> componentIndices;
        for (int i = 0; i < n; i++) {
            componentIndices[dsu.find(i)].push_back(i);
        }
        
        // Result array to store optimized arrangement
        vector<int> result(n);
        
        // For each component, optimize placement
        for (auto& [root, indices] : componentIndices) {
            // Collect values for this component
            vector<int> values;
            for (int idx : indices) {
                values.push_back(nums[idx]);
            }
            
            // Sort values in descending order
            sort(values.begin(), values.end(), greater<int>());
            
            // Separate indices into even and odd
            vector<int> evenIndices, oddIndices;
            for (int idx : indices) {
                if (idx % 2 == 0) {
                    evenIndices.push_back(idx);
                } else {
                    oddIndices.push_back(idx);
                }
            }
            
            // Sort indices
            sort(evenIndices.begin(), evenIndices.end());
            sort(oddIndices.begin(), oddIndices.end());
            
            // Greedy assignment
            int valueIdx = 0;
            for (int idx : evenIndices) {
                result[idx] = values[valueIdx++];
            }
            for (int idx : oddIndices) {
                result[idx] = values[valueIdx++];
            }
        }
        
        // Calculate alternating sum
        long long alternatingSum = 0;
        for (int i = 0; i < n; i++) {
            if (i % 2 == 0) {
                alternatingSum += result[i];
            } else {
                alternatingSum -= result[i];
            }
        }
        
        return alternatingSum;
    }
};
```

---

## Complexity Analysis

- **Time Complexity:** `O(n log n)`  
  - DSU operations: `O(α(n))` (inverse Ackermann, almost constant).  
  - Sorting values for each component: `O(n log n)` in total.  

- **Space Complexity:** `O(n)`  
  - For DSU parent, rank arrays, and temporary storage of components.

---

## Key Takeaways

- The problem is essentially about identifying connected components via swaps.  
- Within each component, indices and values can be rearranged freely.  
- Assigning the largest values to positions with positive contribution (even indices) ensures maximum alternating sum.
