# [62. Unique Paths](https://leetcode.com/problems/unique-paths/)

### Problem:
A robot is placed on an `m x n` grid. Starting from the top-left corner (`grid[0][0]`), the robot aims to reach the bottom-right corner (`grid[m-1][n-1]`).

- The robot can only move either **down** or **right** at any point in time.
- We are tasked with calculating the total number of unique paths the robot can take to reach the destination.
  
The test cases are designed such that the answer will be less than or equal to `2 * 10^9`.

---

### Example 1:
**Input:** `m = 3, n = 7`

**Output:** `28`

### Example 2:
**Input:** `m = 3, n = 2`

**Output:** `3`

**Explanation:**  
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right → Down → Down
2. Down → Down → Right
3. Down → Right → Down

---

### Constraints:
- `1 <= m, n <= 100`

---

## Approach:
This problem can be solved using a **combinatorial approach**:
1. The task of reaching from the top-left corner to the bottom-right corner is equivalent to arranging moves. Specifically:
   - We need `m-1` moves **down** and `n-1` moves **right** to reach the target.
2. This problem can be reduced to a combination problem where we need to choose `m-1` down moves (or `n-1` right moves) from `m+n-2` total moves.
3. The formula to calculate the total unique paths is:
   \[
   \text{Unique Paths} = \binom{N}{r} = \frac{(N)!}{r!(N-r)!}
   \]
   where \( N = m + n - 2 \) and \( r = \min(m-1, n-1) \).
4. We can compute this efficiently using an iterative approach to avoid overflow and unnecessary computation.

---

## Code:

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        long long ans = 1; // Initialize result to store the number of unique paths
        int N = m + n - 2; // Total moves required to reach the bottom-right corner
        int r = min(m - 1, n - 1); // Choose the smaller of (m-1) or (n-1) to minimize calculation

        // Calculate the combination using an iterative approach to avoid large intermediate values
        for (int i = 1; i <= r; ++i) {
            ans = ans * (N - r + i) / i; // Use formula for combinations
        }

        return static_cast<int>(ans); // Return result as an integer
    }
};
