# [Maximize Frequency After Operations I](https://leetcode.com/problems/maximum-frequency-of-an-element-after-performing-operations-i/description/)

**Difficulty**: Medium

## Problem Statement
You are given an integer array `nums` and two integers `k` and `numOperations`. You must perform an operation `numOperations` times on `nums`, where in each operation you:
- Select an index `i` that was not selected in any previous operations.
- Add an integer in the range `[-k, k]` to `nums[i]`.

Return the maximum possible frequency of any element in `nums` after performing the operations.

## Approach
1. **Sorting**: Sort the `nums` array to facilitate range queries and binary searches.
2. **Iterate over Possible Targets**: For each target value in the range `[nums[0] - k, nums[n - 1] + k]`:
   - Use binary search to find the range of elements that can be made equal to the target using `[-k, k]` adjustments.
   - Calculate the current frequency of the target and add additional elements that can be converted to the target, limited by `numOperations`.
3. **Update Maximum Frequency**: Track the maximum frequency achieved across all target values.

## Solution Code

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int maxFrequency(vector<int>& nums, int k, int numOperations) {
        // Sort nums to enable range searches and efficient manipulation
        sort(nums.begin(), nums.end());
        int maxFreq = 1;  // Initial maximum frequency

        // Explore potential target values in the range of [nums[0] - k, nums[n - 1] + k]
        for (int target = nums[0] - k; target <= nums[nums.size() - 1] + k; target++) {
            // `left` points to the first element >= target - k in sorted `nums`
            int left = lower_bound(nums.begin(), nums.end(), target - k) - nums.begin();
            // `right` points to the first element > target + k
            int right = upper_bound(nums.begin(), nums.end(), target + k) - nums.begin();

            // Calculate the frequency of the current target value in nums
            int freq = upper_bound(nums.begin(), nums.end(), target) - lower_bound(nums.begin(), nums.end(), target);

            // Determine the maximum achievable frequency by adding min(numOperations, (right - left - freq))
            int currentFreq = freq + min(numOperations, max(0, right - left - freq));
            
            // Update maxFreq with the maximum achievable frequency for the current target
            maxFreq = max(maxFreq, currentFreq);
        }

        return maxFreq;
    }
};
