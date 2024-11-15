# [3351. Sum of Good Subsequences](https://leetcode.com/problems/sum-of-good-subsequences/)

### Problem Statement:
You are given an integer array `nums`. A **good subsequence** is defined as a subsequence of `nums` where the absolute difference between any two consecutive elements in the subsequence is exactly `1`.

Return the **sum of all possible good subsequences** of `nums`. Since the answer may be very large, return it modulo \(10^9 + 7\).

---

### Examples:

#### Example 1:
**Input:**  
`nums = [1, 2, 1]`  
**Output:**  
`14`  
**Explanation:**  
Good subsequences are: `[1], [2], [1], [1,2], [2,1], [1,2,1]`.  
The sum of elements in these subsequences is `14`.

---

#### Example 2:
**Input:**  
`nums = [3, 4, 5]`  
**Output:**  
`40`  
**Explanation:**  
Good subsequences are: `[3], [4], [5], [3,4], [4,5], [3,4,5]`.  
The sum of elements in these subsequences is `40`.

---

### Constraints:
- \(1 \leq nums.length \leq 10^5\)
- \(0 \leq nums[i] \leq 10^5\)

---

### Code:

```cpp
class Solution {
public:
    int sumOfGoodSubsequences(vector<int>& nums) {
        const int MOD = 1e9 + 7;

        long long frequency[100005] = {0}; // Frequency of numbers
        long long contribution[100005] = {0}; // Contribution of numbers to the sum
        long long totalSum = 0;

        for (int num : nums) {
            frequency[num] = (frequency[num] + 1) % MOD; // Increment frequency of `num`

            int next = num + 1; // Number immediately greater
            int prev = num - 1; // Number immediately smaller
            long long nextContribution = (contribution[next] + num + num * frequency[next]) % MOD;

            contribution[num] = (contribution[num] + nextContribution) % MOD;

            if (prev >= 0) { // If a smaller number exists
                long long prevContribution = (contribution[prev] + num * frequency[prev]) % MOD;
                contribution[num] = (contribution[num] + prevContribution) % MOD;
                frequency[num] = (frequency[num] + frequency[prev]) % MOD;
            }

            frequency[num] = (frequency[num] + frequency[next]) % MOD;
        }

        for (long long contrib : contribution) {
            totalSum = (totalSum + contrib) % MOD;
        }

        return totalSum;
    }
};
