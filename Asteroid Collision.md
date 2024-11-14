# [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)

### Problem:
We are given an array `asteroids` of integers representing asteroids in a row.

- Each asteroid's absolute value represents its size, and the sign represents its direction (`positive` means moving right, `negative` means moving left).
- Asteroids move at the same speed.

Determine the state of the asteroids after all collisions. The collision rules are as follows:
1. If two asteroids meet, the smaller one will explode.
2. If both asteroids are the same size, both will explode.
3. Two asteroids moving in the same direction will never meet.

---

### Example 1:
**Input:** `asteroids = [5,10,-5]`

**Output:** `[5,10]`

**Explanation:**  
The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

### Example 2:
**Input:** `asteroids = [8,-8]`

**Output:** `[]`

**Explanation:**  
The 8 and -8 collide, exploding each other.

### Example 3:
**Input:** `asteroids = [10,2,-5]`

**Output:** `[10]`

**Explanation:**  
The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

---

### Constraints:
- `2 <= asteroids.length <= 10^4`
- `-1000 <= asteroids[i] <= 1000`
- `asteroids[i] != 0`

---

## Approach:
To solve this problem, we can use a stack to simulate the collisions:
1. Iterate over each asteroid in the list.
2. For each asteroid:
   - If it’s moving left (negative) and there's a positive asteroid on the stack, check for collisions.
   - While there’s a possible collision (top of the stack is positive and the current asteroid is negative), compare their sizes:
     - If the incoming asteroid is larger, pop the stack (the top asteroid explodes).
     - If they are the same size, pop the stack and mark the incoming asteroid as exploded.
     - If the incoming asteroid is smaller, mark it as exploded and break out of the loop.
3. If the current asteroid hasn't exploded, push it onto the stack.
4. Finally, convert the stack into the result array.

---

## Code:

```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        stack<int> st;

        for (int asteroid : asteroids) {
            bool exploded = false;

            // Handle collisions
            while (!st.empty() && asteroid < 0 && st.top() > 0) {
                // Compare absolute sizes
                if (abs(asteroid) > abs(st.top())) {
                    st.pop(); // Top asteroid explodes
                } else if (abs(asteroid) == abs(st.top())) {
                    st.pop(); // Both asteroids explode
                    exploded = true;
                    break;
                } else {
                    exploded = true; // Current asteroid explodes
                    break;
                }
            }

            // If the current asteroid didn't explode, push it onto the stack
            if (!exploded) {
                st.push(asteroid);
            }
        }

        // Move remaining asteroids from stack to result vector
        vector<int> result(st.size());
        for (int i = st.size() - 1; i >= 0; --i) {
            result[i] = st.top();
            st.pop();
        }

        return result;
    }
};
