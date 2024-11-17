# [862. Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)

### Problem Statement:
Given an integer array `nums` and an integer `k`, return the length of the shortest non-empty subarray of `nums` with a sum of at least `k`. If there is no such subarray, return `-1`.

A **subarray** is a contiguous part of an array.

---

### Examples:

#### Example 1:
**Input:**  
`nums = [1]`, `k = 1`  
**Output:**  
`1`

#### Example 2:
**Input:**  
`nums = [1, 2]`, `k = 4`  
**Output:**  
`-1`

#### Example 3:
**Input:**  
`nums = [2, -1, 2]`, `k = 3`  
**Output:**  
`3`

---

### Constraints:
- \(1 \leq nums.length \leq 10^5\)
- \(-10^5 \leq nums[i] \leq 10^5\)
- \(1 \leq k \leq 10^9\)

---

### Solution Using Priority Queue

```cpp
class Solution {
public:
    int shortestSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        long long sum = 0;
        int ans = n + 1; // Initialize to an impossible length greater than array size
        priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<pair<long long, int>>> pq;

        for (int i = 0; i < n; i++) {
            sum += nums[i];

            // If the sum itself is greater than or equal to k, update the answer
            if (sum >= k) {
                ans = min(ans, i + 1);
            }

            // Check if removing the smallest prefix results in a valid subarray
            while (!pq.empty() && sum - pq.top().first >= k) {
                ans = min(ans, i - pq.top().second);
                pq.pop();
            }

            // Push the current prefix sum along with its index into the priority queue
            pq.push({sum, i});
        }

        return (ans == n + 1) ? -1 : ans;
    }
};
