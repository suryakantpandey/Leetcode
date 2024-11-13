# [2563. Count the Number of Fair Pairs](https://leetcode.com/problems/count-the-number-of-fair-pairs/)

### Problem:
Given a 0-indexed integer array `nums` of size `n` and two integers `lower` and `upper`, return the number of fair pairs.

A pair `(i, j)` is considered fair if:
- `0 <= i < j < n`, and
- `lower <= nums[i] + nums[j] <= upper`

### Example 1:
**Input:** `nums = [0,1,7,4,4,5], lower = 3, upper = 6`

**Output:** `6`

**Explanation:**  
There are 6 fair pairs: (0,3), (0,4), (0,5), (1,3), (1,4), and (1,5).

### Example 2:
**Input:** `nums = [1,7,9,2,5], lower = 11, upper = 11`

**Output:** `1`

**Explanation:**  
There is a single fair pair: (2,3).

---

## Approach

1. Sort the array `nums` to make it easier to apply binary search techniques to find the range of valid pairs.
2. For each element in `nums`, determine the range of elements that can form a fair pair with it by using:
   - `lower_bound` to find the smallest valid element.
   - `upper_bound` to find the largest valid element.
3. For each valid element found in the range, count it as a fair pair.
4. Adjust the count if a pair includes two identical indices (as they are not allowed).

---

## Code:

```cpp
class Solution {
public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        sort(nums.begin(), nums.end());  // Step 1: Sort the array
        int n = nums.size();
        long long ans = 0;

        for (int i = 0; i < n; ++i) {
            int lx = lower - nums[i];
            int ux = upper - nums[i];

            // Step 2: Find valid range using binary search
            int x = lower_bound(nums.begin() + i, nums.end(), lx) - nums.begin();
            int y = upper_bound(nums.begin() + i, nums.end(), ux) - nums.begin() - 1;

            // Adjust the count of fair pairs if i is part of the pair
            int tmp = y - x + 1;
            if (lx <= nums[i]) {
                tmp--;
            }
            ans += max(0, tmp);
        }
        
        return ans;
    }
};
