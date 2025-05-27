# [2894. Divisible and Non-divisible Sums Difference](https://leetcode.com/problems/divisible-and-non-divisible-sums-difference/)

### Problem:
You are given positive integers `n` and `m`.

Define two integers as follows:
- **num1**: The sum of all integers in the range `[1, n]` (both inclusive) that are **not divisible** by `m`.
- **num2**: The sum of all integers in the range `[1, n]` (both inclusive) that **are divisible** by `m`.

Return the value of `num1 - num2`.

---

### Example 1:
**Input:** `n = 10, m = 3`  
**Output:** `19`  

**Explanation:**  
- Integers not divisible by 3: `[1,2,4,5,7,8,10]` → sum = `37`  
- Integers divisible by 3: `[3,6,9]` → sum = `18`  
Result: `37 - 18 = 19`

---

### Example 2:
**Input:** `n = 5, m = 6`  
**Output:** `15`  

**Explanation:**  
- Not divisible by 6: `[1,2,3,4,5]` → sum = `15`  
- Divisible by 6: `[]` → sum = `0`  
Result: `15 - 0 = 15`

---

### Example 3:
**Input:** `n = 5, m = 1`  
**Output:** `-15`  

**Explanation:**  
- Not divisible by 1: `[]` → sum = `0`  
- Divisible by 1: `[1,2,3,4,5]` → sum = `15`  
Result: `0 - 15 = -15`

---

### Constraints:
- `1 <= n, m <= 1000`

---

## Approach:
We can directly calculate the sum of numbers divisible and not divisible by `m` in the range `[1, n]` using a loop.

However, we can **optimize** this with mathematical formulas:
- The **sum of first n natural numbers** is `S = n * (n + 1) / 2`.
- To find the sum of numbers divisible by `m`:
  - Count of such numbers is `count = n / m`
  - These numbers form an arithmetic progression: `m, 2m, 3m, ..., (n/m)*m`
  - Their sum = `m * (count * (count + 1)) / 2`

So:
- `total_sum = n * (n + 1) / 2`
- `sum_divisible = m * (count * (count + 1)) / 2`
- `sum_not_divisible = total_sum - sum_divisible`
- Return `sum_not_divisible - sum_divisible`

---

## Optimized Code:

```cpp
class Solution {
public:
    int differenceOfSums(int n, int m) {
        int totalSum = n * (n + 1) / 2;
        int count = n / m;
        int divisibleSum = m * (count * (count + 1)) / 2;
        return totalSum - 2 * divisibleSum;
    }
};
```
