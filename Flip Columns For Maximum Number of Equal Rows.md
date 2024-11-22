# [1072. Flip Columns For Maximum Number of Equal Rows](https://leetcode.com/problems/flip-columns-for-maximum-number-of-equal-rows/)

### Problem Statement:
You are given an `m x n` binary matrix `matrix`.

You can choose any number of columns in the matrix and flip every cell in that column (i.e., change the value of the cell from `0` to `1` or vice versa).

Return the maximum number of rows that have all values equal after some number of flips.

---

### Examples:

#### Example 1:
**Input:**  
`matrix = [[0,1],[1,1]]`  
**Output:**  
`1`  
**Explanation:**  
After flipping no values, 1 row has all values equal.

#### Example 2:
**Input:**  
`matrix = [[0,1],[1,0]]`  
**Output:**  
`2`  
**Explanation:**  
After flipping values in the first column, both rows have equal values.

#### Example 3:
**Input:**  
`matrix = [[0,0,0],[0,0,1],[1,1,0]]`  
**Output:**  
`2`  
**Explanation:**  
After flipping values in the first two columns, the last two rows have equal values.

---

### Constraints:
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 300`
- `matrix[i][j]` is either `0` or `1`.

---

### Approach:

1. **Canonical Representation of Rows:**
   - Each row can be represented in a canonical form such that flipping columns will not affect its representation.
   - Convert each row to a string where the first element determines the transformation:  
     - If a cell value equals the first value of the row, append `'0'`.  
     - Otherwise, append `'1'`.

2. **Count Patterns:**
   - Use a hash map to count the frequency of each canonical pattern.  
     Rows with the same canonical pattern can be made identical by flipping appropriate columns.

3. **Find the Maximum Rows:**
   - The maximum value in the hash map represents the maximum number of rows that can be made equal.

---

### Code Implementation:

```cpp
class Solution {
public:
    int maxEqualRowsAfterFlips(vector<vector<int>>& matrix) {
        unordered_map<string, int> patternCount; // Stores frequency of each canonical pattern
        int m = matrix.size(), n = matrix[0].size();
        
        // Generate the canonical form for each row
        for (int i = 0; i < m; ++i) {
            string canonical;
            for (int j = 0; j < n; ++j) {
                // Append '0' if the value matches the first element, otherwise '1'
                canonical += (matrix[i][j] == matrix[i][0] ? '0' : '1');
            }
            patternCount[canonical]++; // Count the frequency of the pattern
        }
        
        int maxRows = 0;
        // Find the maximum frequency of any pattern
        for (auto& [pattern, count] : patternCount) {
            maxRows = max(maxRows, count);
        }
        
        return maxRows; // Maximum number of rows that can be made identical
    }
};
