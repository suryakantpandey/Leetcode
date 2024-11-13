# [1266. Minimum Time Visiting All Points](https://leetcode.com/problems/minimum-time-visiting-all-points/)

### Problem:
On a 2D plane, there are `n` points with integer coordinates `points[i] = [xi, yi]`. Return the minimum time in seconds to visit all the points in the order given by `points`.

You can move according to these rules:

In 1 second, you can either:
- Move vertically by one unit.
- Move horizontally by one unit, or
- Move diagonally sqrt(2) units (in other words, move one unit vertically then one unit horizontally in 1 second).

You have to visit the points in the same order as they appear in the array.
You are allowed to pass through points that appear later in the order, but these do not count as visits.

### Example 1:
**Input:** `points = [[1,1],[3,4],[-1,0]]`

**Output:** `7`

**Explanation:** One optimal path is:
[1,1] → [2,2] → [3,3] → [3,4] → [2,3] → [1,2] → [0,1] → [-1,0]

Time from [1,1] to [3,4] = 3 seconds  
Time from [3,4] to [-1,0] = 4 seconds  
Total time = 7 seconds

### Example 2:
**Input:** `points = [[3,2],[-2,2]]`

**Output:** `5`

---

## Approach

The approach involves calculating the time to move from one point to the next. The time is determined by the maximum of the horizontal and vertical distances between two consecutive points because diagonal movement can reduce both distances simultaneously.

- The function `findMinTimeToVisit` calculates the time to move from one point to another by taking the maximum of the absolute differences in x and y coordinates (which corresponds to moving diagonally if possible).
- We then iterate through the list of points and compute the total time.

---

## Code:

```cpp
class Solution {
public:
    int minTimeToVisitAllPoints(vector<vector<int>>& points) {
        int minTimeTaken = 0;
        vector<int> start;
        for(auto point : points){
            if(!start.empty()){
                minTimeTaken += findMinTimeToVisit(start , point);
            }
            start = point;   
        }
        return minTimeTaken;
    }

    int findMinTimeToVisit(vector<int>& start , vector<int>& end) {
        int horizontalDistance = abs(end.front() - start.front());
        int verticalDistance = abs(end.back() - start.back());
        return max(horizontalDistance , verticalDistance);
    }
};
