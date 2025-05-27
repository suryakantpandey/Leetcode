# 3502. Minimum Cost to Reach Every Position(https://leetcode.com/problems/minimum-cost-to-reach-every-position/)

**Difficulty**: Easy  
**Topics**: Array, Greedy

---

## Problem Statement

You are given an integer array `cost` of size `n`. You are currently at position `n` (at the end of the line) in a line of `n + 1` people (numbered from `0` to `n`).

You wish to move forward in the line, but each person in front of you charges a specific amount to swap places. The cost to swap with person `i` is given by `cost[i]`.

You are allowed to swap places with people as follows:

- If they are in front of you, you must pay them `cost[i]` to swap with them.
- If they are behind you, they can swap with you for free.

Return an array `answer` of size `n`, where `answer[i]` is the minimum total cost to reach each position `i` in the line.

---

## Example 1

**Input**: `cost = [5,3,4,1,3,2]`  
**Output**: `[5,3,3,1,1,1]`  

**Explanation**:
- `i = 0`: Swap with person 0 for cost 5.
- `i = 1`: Swap with person 1 for cost 3.
- `i = 2`: Swap with person 1 (3), then swap for free with person 2.
- `i = 3`: Swap with person 3 for cost 1.
- `i = 4`: Swap with person 3 (1), then swap for free with person 4.
- `i = 5`: Swap with person 3 (1), then swap for free with person 5.

---

## Example 2

**Input**: `cost = [1,2,4,6,7]`  
**Output**: `[1,1,1,1,1]`

---

## Constraints

- `1 <= n == cost.length <= 100`
- `1 <= cost[i] <= 100`

---

## Optimized C++ Solution

```cpp
class Solution {
public:
    vector<int> minCosts(vector<int>& cost) {
        int n = cost.size();
        vector<int> ans(n, 0);
        int mn = cost[0];

        for (int i = 0; i < n; i++) {
            ans[i] = min(cost[i], mn);
            mn = min(mn, cost[i]);
        }

        return ans;
    }
};
```

**Time Complexity**: `O(n)`  
**Space Complexity**: `O(n)`

---
