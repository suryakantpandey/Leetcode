# [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

### Problem Statement:
Given an unsorted integer array `nums`, return the smallest positive integer that is not present in the array.

You must implement an algorithm that runs in **O(n)** time and uses **O(1)** auxiliary space.

---

### Examples:

#### Example 1:
**Input:**  
`nums = [1, 2, 0]`  
**Output:**  
`3`  
**Explanation:**  
- Numbers in the range `[1, 2]` are all present. The smallest missing positive integer is `3`.

#### Example 2:
**Input:**  
`nums = [3, 4, -1, 1]`  
**Output:**  
`2`  
**Explanation:**  
- `1` is in the array, but `2` is missing.

#### Example 3:
**Input:**  
`nums = [7, 8, 9, 11, 12]`  
**Output:**  
`1`  
**Explanation:**  
- The smallest missing positive integer is `1`.

---

### Constraints:
- `1 <= nums.length <= 10^5`
- `-2^31 <= nums[i] <= 2^31 - 1`

---

### Solution

#### Code Implementation:

```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();  // Size of the input array
        
        // Step 1: Replace invalid numbers (negative numbers or numbers greater than n) with 0
        for (int i = 0; i < n; i++) {
            if (nums[i] <= 0 || nums[i] > n) {
                nums[i] = 0;  // Replace invalid values with 0
            }
        }
        
        // Add a placeholder value at the end to handle edge cases
        nums.push_back(0);

        // Step 2: Use the array itself as a hash table
        // Mark presence of a number by adding `n + 1` to the index corresponding to the value
        for (int i = 0; i < n; i++) {
            int val = nums[i] % (n + 1);  // Extract the original value at index `i`
            if (val > 0) {  // Only mark for positive numbers
                nums[val] += (n + 1);  // Increment at the index `val` by `n + 1`
            }
        }

        // Step 3: Identify the first missing positive integer
        // Check the array for the first index where the value is less than `n + 1`
        for (int i = 1; i <= n; i++) {
            if (nums[i] / (n + 1) == 0) {  // If count at index `i` is still 0
                return i;  // `i` is the first missing positive integer
            }
        }

        // If all numbers from 1 to n are present, the missing number is `n + 1`
        return n + 1;
    }
};
