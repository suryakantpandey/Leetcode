# [2064. Minimized Maximum of Products Distributed to Any Store](https://leetcode.com/problems/minimized-maximum-of-products-distributed-to-any-store/)

### Problem:
You are given an integer `n`, representing the number of specialty retail stores, and an integer array `quantities` where `quantities[i]` is the number of products of the `i-th` product type.

You need to distribute all products to the retail stores following these rules:
- A store can only be given one product type but any amount of it.
- After distribution, let `x` represent the maximum number of products assigned to any store. We aim to minimize `x`.

Return the minimum possible value of `x`.

---

### Example 1:
**Input:** `n = 6`, `quantities = [11,6]`

**Output:** `3`

**Explanation:**  
The 11 products of type 0 are distributed to the first four stores as follows: `[2, 3, 3, 3]`  
The 6 products of type 1 are distributed to the remaining two stores: `[3, 3]`  
The maximum products any store has is `3`.

### Example 2:
**Input:** `n = 7`, `quantities = [15,10,10]`

**Output:** `5`

### Example 3:
**Input:** `n = 1`, `quantities = [100000]`

**Output:** `100000`

---

### Constraints:
- `m == quantities.length`
- `1 <= m <= n <= 10^5`
- `1 <= quantities[i] <= 10^5`

---

## Approach:
The task is to find the minimum possible `x` using binary search on the potential values of `x`. The approach can be broken down as follows:

1. **Binary Search Setup**:
   - Define the minimum (`l`) and maximum (`r`) possible values for `x`.
   - Initialize `l` to 1 and `r` to the maximum quantity in `quantities`.
   
2. **Binary Search**:
   - For each midpoint `m`, determine if it's possible to distribute the products such that no store has more than `m` items using a helper function `check`.
   - If `check` returns `true` (feasible within `n` stores), reduce the upper bound `r` to `m - 1` and update `ans`.
   - Otherwise, increase the lower bound `l` to `m + 1`.

3. **Helper Function (`check`)**:
   - Calculate the number of stores required if each store has at most `maxCap` items. If this number exceeds `n`, return `false`.

This binary search technique ensures that we find the minimized maximum distribution.

---

## Code:

```cpp
class Solution {
public:
    // Helper function to determine if we can distribute with given max capacity
    bool check(const vector<int>& quantities, int maxCap, int n) {
        int storesNeeded = 0;
        for (int quantity : quantities) {
            storesNeeded += (quantity + maxCap - 1) / maxCap;  // Calculate required stores
            if (storesNeeded > n) {
                return false;  // Early exit if stores needed exceed available stores
            }
        }
        return true;
    }

    int minimizedMaximum(int n, vector<int>& quantities) {
        int l = 1;  // Minimum possible capacity
        int r = *max_element(quantities.begin(), quantities.end());  // Maximum capacity
        int ans = r;
        
        // Binary search on the capacity to minimize the maximum load per store
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (check(quantities, m, n)) {  // Check feasibility with current max capacity
                ans = m;  // Update answer if feasible
                r = m - 1;  // Try to find a smaller feasible max capacity
            } else {
                l = m + 1;  // Increase lower bound to search for feasible capacity
            }
        }
        return ans;
    }
};
