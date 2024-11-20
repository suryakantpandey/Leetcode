# [2516. Take K of Each Character From Left and Right](https://leetcode.com/problems/take-k-of-each-character-from-left-and-right/)

### Problem Statement:
You are given a string `s` consisting of the characters `'a'`, `'b'`, and `'c'` and a non-negative integer `k`. Each minute, you may take either the leftmost or the rightmost character of `s`.  

Return the **minimum number of minutes** needed to take **at least k of each character**, or return `-1` if it is not possible to take `k` of each character.

---

### Examples:

#### Example 1:
**Input:**  
`s = "aabaaaacaabc", k = 2`  
**Output:**  
`8`  
**Explanation:**  
- Take 3 characters from the left (`a`, `a`, `b`): We now have 2 `'a'`s and 1 `'b'`.  
- Take 5 characters from the right (`a`, `c`, `a`, `a`, `b`): We now have 4 `'a'`s, 2 `'b'`s, and 2 `'c'`s.  
- Total time: `3 + 5 = 8` minutes.

#### Example 2:
**Input:**  
`s = "a", k = 1`  
**Output:**  
`-1`  
**Explanation:**  
It is impossible to take at least `1` `'b'` or `'c'`.  

---

### Constraints:
- \(1 \leq s.length \leq 10^5\)
- \(s\) consists only of the letters `'a'`, `'b'`, and `'c'`.
- \(0 \leq k \leq s.length\)

---

### Approach:

To solve the problem efficiently, we use the **sliding window technique**:
1. **Precondition Check:**  
   - Count the total frequency of `'a'`, `'b'`, and `'c'`.  
   - If any character occurs less than `k` times, return `-1` immediately.

2. **Sliding Window:**  
   - Use a two-pointer technique to find the **longest subsequence** (middle portion of `s`) that can be removed while ensuring that the remaining string contains at least `k` of each character.

3. **Result Calculation:**  
   - The **minimum minutes** required is the total string length minus the size of the valid removable subsequence.

---

### Code Implementation:

```cpp
class Solution {
public:
    int takeCharacters(string s, int k) {
        if (k == 0) return 0; // No characters need to be removed if k is 0.

        vector<int> freq(3, 0); // `freq` tracks overall counts of 'a', 'b', 'c'.

        // Calculate the total frequency of each character.
        for (char ch : s) freq[ch - 'a']++;

        // If any character appears less than k times, it's impossible to satisfy the condition.
        if (any_of(freq.begin(), freq.end(), [&](int f) { return f < k; })) return -1;

        int j = 0, maxSize = 0, n = s.size();

        // Sliding window to find the largest removable subsequence.
        for (int i = 0; i < n; i++) {
            freq[s[i] - 'a']--; // Decrease count of the current character in the remaining portion.

            // Shrink the window until the remaining string satisfies the condition for all characters.
            while (freq[0] < k || freq[1] < k || freq[2] < k) {
                freq[s[j++] - 'a']++; // Restore the character count as we shrink the window.
            }

            // Update the maximum size of a valid subsequence.
            maxSize = max(maxSize, i - j + 1);
        }

        // Return the minimum number of characters to remove.
        return n - maxSize;
    }
};
