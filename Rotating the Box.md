# [1861. Rotating the Box](https://leetcode.com/problems/rotating-the-box/)

## Problem Statement

You are given an \( m \times n \) matrix of characters `box` representing a side-view of a box. Each cell of the box is one of the following:

- A stone (`'#'`)
- A stationary obstacle (`'*'`)
- Empty (`'.'`)

The box is rotated 90 degrees clockwise, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity does not affect the obstacles' positions, and the inertia from the box's rotation does not affect the stones' horizontal positions.

It is guaranteed that each stone in the box rests on an obstacle, another stone, or the bottom of the box.

Return an \( n \times m \) matrix representing the box after the rotation described above.

---

## Examples

### Example 1

**Input:**  
`box = [["#",".","#"]]`

**Output:**  
`[["."], ["#"], ["#"]]`

---

### Example 2

**Input:**  
`box = [["#",".","*","."], ["#","#","*","."]]`

**Output:**  
`[["#","."], ["#","#"], ["*","*"], [".","."]]`

---

### Example 3

**Input:**  
`box = [["#","#","*",".","*","."], ["#","#","#","*",".","."], ["#","#","#",".","#","."]]`

**Output:**  
`[[".","#","#"], [".","#","#"], ["#","#","*"], ["#","*","."], ["#",".","*"], ["#",".","."]]`

---

## Constraints

- \( m = \text{box.length} \)
- \( n = \text{box[i].length} \)
- \( 1 \leq m, n \leq 500 \)
- \( \text{box[i][j]} \in \{'#', '*', '.'\} \)

---

## Intuition

### Key Observations

1. **Gravity Simulation:**
   - Stones (`'#'`) fall downward until they hit an obstacle (`'*'`), another stone, or the bottom of the box.

2. **Rotation Process:**
   - After applying gravity to each row, the box is rotated 90 degrees clockwise:
     - The \( i^{th} \) row becomes the \( m-1-i^{th} \) column.

### Step-by-Step Approach

1. **Simulate Gravity:**
   - For each row:
     - Traverse from right to left to identify where each stone should fall.
     - Keep track of the lowest empty position (`emptyPos`) and place stones as they "fall" downward.

2. **Rotate the Box:**
   - Create a new matrix of size \( n \times m \).
   - After processing each row, update the corresponding column in the rotated matrix.

---

## Approach

### Steps

1. Traverse each row of the box to apply gravity:
   - Keep track of the lowest empty position for stones (`emptyPos`).
   - When a stone (`'#'`) is encountered, move it to the lowest position.
   - When an obstacle (`'*'`) is encountered, update the empty position below it.

2. Populate the rotated matrix:
   - Place each element from the processed rows into its new position in the rotated matrix.

---

## Code Implementation

```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& box) {
        int m = box.size();    // Rows in original box
        int n = box[0].size(); // Columns in original box
        
        // Create rotated box of size n x m
        vector<vector<char>> rotatedBox(n, vector<char>(m, '.'));

        for (int i = 0; i < m; i++) {
            int emptyPos = n - 1; // Track the lowest empty position for gravity
            for (int j = n - 1; j >= 0; j--) {
                if (box[i][j] == '*') {
                    // Obstacle remains fixed
                    rotatedBox[j][m - 1 - i] = '*';
                    emptyPos = j - 1; // Reset empty position below the obstacle
                } else if (box[i][j] == '#') {
                    // Place stone at the lowest available position
                    rotatedBox[emptyPos--][m - 1 - i] = '#';
                }
            }
        }

        return rotatedBox;
    }
};
