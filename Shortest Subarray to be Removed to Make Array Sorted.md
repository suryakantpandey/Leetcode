# [1574. Shortest Subarray to be Removed to Make Array Sorted](https://leetcode.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)

### Problem:
Given an integer array `arr`, remove a subarray (can be empty) such that the remaining elements in `arr` are in non-decreasing order.

Return the **length of the shortest subarray** to remove.

A subarray is a contiguous subsequence of the array.

---

### Example 1:
**Input:**  
`arr = [1,2,3,10,4,2,3,5]`  
**Output:**  
`3`  
**Explanation:**  
The shortest subarray we can remove is `[10,4,2]` of length 3. The remaining elements will be `[1,2,3,3,5]`, which are sorted.  
Another correct solution is to remove the subarray `[3,10,4]`.

---

### Example 2:
**Input:**  
`arr = [5,4,3,2,1]`  
**Output:**  
`4`  
**Explanation:**  
Since the array is strictly decreasing, we can only keep a single element. Therefore, we need to remove a subarray of length 4, either `[5,4,3,2]` or `[4,3,2,1]`.

---

### Example 3:
**Input:**  
`arr = [1,2,3]`  
**Output:**  
`0`  
**Explanation:**  
The array is already non-decreasing. We do not need to remove any elements.

---

### Constraints:
- ( 1 < arr.length < 10^5 )
- ( 0 < arr[i] < 10^9 )

---

### Approach:

1. **Identify Prefix and Suffix:**  
   - Find the longest prefix of `arr` that is already sorted (`left`).
   - Find the longest suffix of `arr` that is already sorted (`right`).

2. **Special Case:**  
   - If the entire array is sorted, return `0`.

3. **Merge Prefix and Suffix:**  
   - Attempt to merge the prefix and suffix by iterating over their indices and checking if elements in the prefix can connect to elements in the suffix.

4. **Return the Minimum:**  
   - The shortest subarray to remove is the minimum of:
     - Removing the prefix (`n - left - 1`).
     - Removing the suffix (`right`).
     - Removing the elements between the prefix and suffix (`j - i - 1`).

---

### Code:

```cpp
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int n = arr.size();
        int left = 0, right = n - 1;

        // Find the length of the increasing prefix
        while (left + 1 < n && arr[left] <= arr[left + 1]) {
            left++;
        }

        // If the array is already sorted, no need to remove any subarray
        if (left == n - 1) return 0;

        // Find the length of the increasing suffix
        while (right > 0 && arr[right - 1] <= arr[right]) {
            right--;
        }

        // Minimum subarray to remove is either the prefix or suffix
        int result = min(n - left - 1, right);

        // Check for merging the prefix and suffix
        int i = 0, j = right;
        while (i <= left && j < n) {
            if (arr[i] <= arr[j]) {
                result = min(result, j - i - 1);
                i++;
            } else {
                j++;
            }
        }

        return result;
    }
};
