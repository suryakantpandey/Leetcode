# LeetCode Solutions Repository

Welcome to my LeetCode repository! This collection is a compilation of solutions to various LeetCode problems, aiming for minimal and efficient code. Each solution is designed to be as concise as possible while maintaining clarity and optimal performance.

## Repository Structure

- **Organized by Difficulty**: Problems are grouped into folders based on difficulty level - `Easy`, `Medium`, and `Hard`.
- **Solution Format**: Each solution file contains:
  - **Problem Statement**: A brief description of the problem.
  - **Approach**: A minimized approach explaining the core idea and key steps.
  - **Code**: Optimized, well-documented code for readability.
  - **Complexity Analysis**: Time and space complexity of the solution.
  
## Goals of This Repository

- Provide clear and efficient solutions for a wide range of LeetCode problems.
- Maintain a minimalistic coding approach to focus on solving problems effectively.
- Serve as a quick reference for interview preparation.

## Solution Structure Example

Each problem solution follows a similar structure for consistency and ease of understanding:

Problem: Two Sum (Example)
Difficulty: Easy

### Problem Statement
Given an array of integers `nums` and an integer `target`, return the indices of the two numbers that add up to the `target`.

### Approach
1. Use a hash map to store the difference between the target and each element as we iterate.
2. Check if the required number exists in the hash map.

### Code
```java
// Java code example
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int diff = target - nums[i];
        if (map.containsKey(diff)) {
            return new int[] { map.get(diff), i };
        }
        map.put(nums[i], i);
    }
    return new int[] {}; // Edge case if no solution found
}

