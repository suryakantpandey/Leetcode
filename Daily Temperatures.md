# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

### Problem Statement:
Given an array of integers `temperatures` representing the daily temperatures, return an array `answer` such that:
- `answer[i]` is the number of days you have to wait after the `i-th` day to get a warmer temperature.
- If there is no future day for which this is possible, set `answer[i] = 0`.

---

### Examples:

#### Example 1:
**Input:**  
`temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`  
**Output:**  
`[1, 1, 4, 2, 1, 1, 0, 0]`  
**Explanation:**  
- For `73` at index 0, the next warmer temperature is `74` after 1 day.
- For `74` at index 1, the next warmer temperature is `75` after 1 day.
- For `75` at index 2, the next warmer temperature is `76` after 4 days.
- For the last two temperatures, there are no warmer days.

#### Example 2:
**Input:**  
`temperatures = [30, 40, 50, 60]`  
**Output:**  
`[1, 1, 1, 0]`  
**Explanation:**  
- Each temperature has a warmer day the following day, except for the last day.

#### Example 3:
**Input:**  
`temperatures = [30, 60, 90]`  
**Output:**  
`[1, 1, 0]`  
**Explanation:**  
- For `30`, the next warmer temperature is `60` after 1 day.
- For `60`, the next warmer temperature is `90` after 1 day.
- For `90`, there is no warmer day.

---

### Constraints:
- `1 <= temperatures.length <= 10^5`
- `30 <= temperatures[i] <= 100`

---

### Solution

#### Code Implementation:

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();            // Size of the input array
        vector<int> result(n, 0);               // Initialize the result array with zeros
        stack<int> indicesStack;                // Stack to store indices of unresolved temperatures

        // Iterate through the temperatures
        for (int currentDay = 0; currentDay < n; currentDay++) {
            // Check if the current temperature resolves any previous colder temperatures
            while (!indicesStack.empty() && temperatures[indicesStack.top()] < temperatures[currentDay]) {
                int previousDay = indicesStack.top();  // Get the index of the colder day
                indicesStack.pop();                    // Remove it from the stack
                result[previousDay] = currentDay - previousDay;  // Calculate the days difference
            }
            indicesStack.push(currentDay);  // Add the current day's index to the stack
        }

        return result;  // Return the result array
    }
};
