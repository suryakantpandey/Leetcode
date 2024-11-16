# [3254. Find the Power of K-Size Subarrays I](https://leetcode.com/problems/find-the-power-of-k-size-subarrays-i/)

### Problem Statement:
You are given an array of integers `nums` of length `n` and a positive integer `k`.

The **power** of an array is defined as:
- Its maximum element if all of its elements are consecutive and sorted in ascending order.
- \(-1\) otherwise.

You need to find the power of all **subarrays** of `nums` of size `k`.

Return an integer array `results` of size \(n - k + 1\), where `results[i]` is the power of `nums[i..(i + k - 1)]`.

---

### Examples:

#### Example 1:
**Input:**  
`nums = [1, 2, 3, 4, 3, 2, 5]`, `k = 3`  
**Output:**  
`[3, 4, -1, -1, -1]`  
**Explanation:**  
The subarrays of size `k = 3` are:  
- `[1, 2, 3]` → Valid (strictly increasing, max = 3).  
- `[2, 3, 4]` → Valid (strictly increasing, max = 4).  
- `[3, 4, 3]` → Invalid (not strictly increasing).  
- `[4, 3, 2]` → Invalid (not sorted).  
- `[3, 2, 5]` → Invalid (not strictly increasing).  

#### Example 2:
**Input:**  
`nums = [2, 2, 2, 2, 2]`, `k = 4`  
**Output:**  
`[-1, -1]`  
**Explanation:**  
None of the subarrays of size `4` are strictly increasing.

#### Example 3:
**Input:**  
`nums = [3, 2, 3, 2, 3, 2]`, `k = 2`  
**Output:**  
`[-1, 3, -1, 3, -1]`  

---

### Constraints:
- (1 <= n = nums.length <= 500)
- (1 <= nums[i] <= 10^5)
- (1 <= k <= n)

---

### Code:

```cpp
class Solution {
public:
    vector<int> resultsArray(vector<int>& nums, int k) {
        vector<int> ans; // To store the resulting values
        int n = nums.size();
        
        // Edge case: If the array has one element or k = 1, return the array itself
        if (n == 1 || k == 1) {
            return nums;
        }

        // Edge case: If k is greater than the array size, return an empty result
        if (k > n) {
            return {};
        }

        // Iterate through each possible subarray of size k
        for (int i = 0; i <= n - k; i++) {
            bool isValid = true; // Flag to check if the subarray is strictly increasing
            int maxValue = nums[i]; // Initialize max value as the first element of the subarray
            
            // Check the subarray from index i to i + k - 1
            for (int j = i + 1; j < i + k; j++) {
                if (nums[j] != nums[j - 1] + 1) {
                    isValid = false; // If not strictly increasing, mark as invalid
                    break;
                }
                maxValue = max(maxValue, nums[j]); // Update the maximum value
            }

            // If the subarray is valid, append the maximum value; otherwise, append -1
            ans.push_back(isValid ? maxValue : -1);
        }
        
        return ans;
    }
};
