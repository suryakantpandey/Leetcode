# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

### Problem Statement:
You are given an integer array `nums` and an integer `k`. Find the maximum subarray sum of all the subarrays of `nums` that meet the following conditions:

1. The length of the subarray is `k`.
2. All the elements of the subarray are distinct.

Return the maximum subarray sum of all the subarrays that meet the conditions. If no subarray meets the conditions, return `0`.

A subarray is a contiguous non-empty sequence of elements within an array.

---

### Examples:

#### Example 1:
**Input:**  
`nums = [1,5,4,2,9,9,9], k = 3`  
**Output:**  
`15`  
**Explanation:**  
- The subarrays of `nums` with length 3 are:
  - `[1,5,4]` which meets the requirements and has a sum of `10`.
  - `[5,4,2]` which meets the requirements and has a sum of `11`.
  - `[4,2,9]` which meets the requirements and has a sum of `15`.
  - `[2,9,9]` which does not meet the requirements because the element `9` is repeated.
  - `[9,9,9]` which does not meet the requirements because the element `9` is repeated.

We return `15` because it is the maximum subarray sum of all the subarrays that meet the conditions.

#### Example 2:
**Input:**  
`nums = [4,4,4], k = 3`  
**Output:**  
`0`  
**Explanation:**  
- The subarrays of `nums` with length 3 are:
  - `[4,4,4]` which does not meet the requirements because the element `4` is repeated.

We return `0` because no subarrays meet the conditions.

---

### Constraints:
- 1 ≤ k ≤ nums.length ≤ 10^5
- 1 ≤ nums[i] ≤ 10^5

---

### Approach:

To solve this problem efficiently, we use the **sliding window technique** combined with a hash map to track the frequency of elements within the window. The steps are:

1. **Initialize the First Window:**
   - Set up a frequency map (`unordered_map`) to track how many times each element appears in the window.
   - Calculate the sum of the first `k` elements.

2. **Check for Unique Elements:**
   - If the frequency map contains exactly `k` distinct elements, update the maximum sum.

3. **Slide the Window:**
   - Move the window one element to the right by subtracting the element that is left behind and adding the new element.
   - If the window still contains exactly `k` unique elements, update the maximum sum.

4. **Return the Result:**
   - If no valid subarray is found, return `0`. Otherwise, return the maximum subarray sum found.

---

### Code Implementation:

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> freq; // Tracks frequency of elements in the current window.
        long long sum = 0, maxSum = 0;

        // Initialize the first window and calculate its sum.
        for (int i = 0; i < k; i++) {
            freq[nums[i]]++;
            sum += nums[i];
        }
        if (freq.size() == k) maxSum = sum; // Check if all elements are unique.

        // Slide the window across the array.
        for (int i = k; i < nums.size(); i++) {
            sum += nums[i] - nums[i - k]; // Update the sum for the new window.
            if (--freq[nums[i - k]] == 0) freq.erase(nums[i - k]); // Remove outgoing element.
            freq[nums[i]]++; // Add incoming element.

            // Update the maximum sum if the window contains all unique elements.
            if (freq.size() == k) maxSum = max(maxSum, sum);
        }

        return maxSum; // Return the maximum valid subarray sum.
    }
};
