# [1652. Defuse the Bomb](https://leetcode.com/problems/defuse-the-bomb/)

### Problem Statement:
You are provided with a circular array `code` of length `n` and an integer key `k`. To decrypt the code:
1. If `k > 0`, replace the `i-th` number with the sum of the next `k` numbers.
2. If `k < 0`, replace the `i-th` number with the sum of the previous `|k|` numbers.
3. If `k == 0`, replace the `i-th` number with `0`.

The array is circular, meaning:
- The next element of `code[n-1]` is `code[0]`.
- The previous element of `code[0]` is `code[n-1]`.

Return the decrypted code.

---

### Examples:

#### Example 1:
**Input:**  
`code = [5, 7, 1, 4]`, `k = 3`  
**Output:**  
`[12, 10, 16, 13]`  
**Explanation:**  
- Decrypted code: `[7+1+4, 1+4+5, 4+5+7, 5+7+1]`.

#### Example 2:
**Input:**  
`code = [1, 2, 3, 4]`, `k = 0`  
**Output:**  
`[0, 0, 0, 0]`  
**Explanation:**  
- `k = 0`: Replace all numbers with `0`.

#### Example 3:
**Input:**  
`code = [2, 4, 9, 3]`, `k = -2`  
**Output:**  
`[12, 5, 6, 13]`  
**Explanation:**  
- Decrypted code: `[3+9, 2+3, 4+2, 9+4]`.

---

### Constraints:
- `n == code.length`
- `1 <= n <= 100`
- `1 <= code[i] <= 100`
- `-(n - 1) <= k <= n - 1`

---

### Solution

#### Code Implementation:

```cpp
class Solution {
public:
    vector<int> decrypt(vector<int>& code, int k) {
        int n = code.size();
        vector<int> decrypted(n, 0); // Initialize the decrypted array with zeros

        // If k == 0, return an array filled with zeros
        if (k == 0) {
            return decrypted;
        }

        // Determine the direction and magnitude of the shift
        int start = (k > 0) ? 1 : n + k; // Start index based on positive or negative k
        int end = (k > 0) ? k : n - 1;  // End index based on positive or negative k
        k = abs(k); // Use the absolute value of k for simplicity

        // Calculate the sliding window sum for the first element
        int windowSum = 0;
        for (int i = start; i <= end; i++) {
            windowSum += code[i % n]; // Handle circular indexing
        }

        // Decrypt the array using a sliding window approach
        for (int i = 0; i < n; i++) {
            decrypted[i] = windowSum; // Assign the current window sum

            // Update the sliding window: subtract the element sliding out and add the new element
            windowSum -= code[(start++) % n]; // Remove the outgoing element
            windowSum += code[(++end) % n];  // Add the incoming element
        }

        return decrypted;
    }
};
