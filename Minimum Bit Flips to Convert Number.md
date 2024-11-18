# [2220. Minimum Bit Flips to Convert Number](https://leetcode.com/problems/minimum-bit-flips-to-convert-number/)

### Problem Statement:
A **bit flip** of a number `x` is the operation of flipping a bit in the binary representation of `x` from `0` to `1` or `1` to `0`.

Given two integers `start` and `goal`, return the **minimum number of bit flips** required to convert `start` to `goal`.

---

### Examples:

#### Example 1:
**Input:**  
start = 10, goal = 7  
**Output:**  
3  
**Explanation:**  
- Binary representations:  
  - start = 10 → 1010  
  - goal = 7 → 0111  
- Steps to convert:  
  - Flip the 1st bit (rightmost): 1010 → 1011.  
  - Flip the 3rd bit: 1011 → 1111.  
  - Flip the 4th bit: 1111 → 0111.  

#### Example 2:
**Input:**  
start = 3, goal = 4  
**Output:**  
3  
**Explanation:**  
- Binary representations:  
  - start = 3 → 011  
  - goal = 4 → 100  
- Steps to convert:  
  - Flip the 1st bit: 011 → 010.  
  - Flip the 2nd bit: 010 → 000.  
  - Flip the 3rd bit: 000 → 100.  

---

### Constraints:
- 0 ≤ start, goal ≤ 10<sup>9</sup>

---

### Solution

The key idea is to use the **XOR operation**:
1. XOR `start` and `goal` to identify the bits that differ.
2. Count the number of `1`s in the XOR result, as each `1` corresponds to a required flip.

#### Code Implementation:

```cpp
class Solution {
public:
    int minBitFlips(int start, int goal) {
        int xorResult = start ^ goal; // XOR identifies differing bits
        int countBits = 0;
        
        // Count the number of 1s in xorResult
        while (xorResult > 0) {
            countBits += xorResult & 1; // Check the least significant bit
            xorResult >>= 1;            // Right shift to process the next bit
        }
        
        return countBits;
    }
};
