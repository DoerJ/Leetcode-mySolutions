# leetcode-mySolutions
My solutions to easy &amp; medium challenges
## Algorithms and Examples

### Backtracking Searching(DFS)
An algorithm that iterates through all the possible solution. Suitable for problems: permutations

Example: Permutation
```javascript
/**
Given a collection of distinct integers, return all possible permutations.
Example:
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function(nums) {
    var sol = new Array();
    if(nums.length === 0) {
        return sol;
    }
    else {
        var i;
        for(i = 0; i < nums.length; i++) {
            var sub_sol = new Array();
            sub_sol.push(nums[i]);
            var remainings = nums.slice();
            remainings.splice(i, 1);
            backtrackingSearch(sub_sol, remainings, sol);
        }
    }
    return sol;
};

var backtrackingSearch = function(sub_sol, remainings, sol) {
    // base case(the bottom level of search tree)
    if(remainings.length === 0) {
        sol.push(sub_sol);
    }
    else {
        var j;
        for(j = 0; j < remainings.length; j++) {
            var sub_sols = sub_sol.slice();
            sub_sols.push(remainings[j]);
            var sub_remainings = remainings.slice();
            sub_remainings.splice(j, 1);
            backtrackingSearch(sub_sols, sub_remainings, sol);
        }
    }
}

```

### Binary Search(half-interval search)
The search algorithm that finds the position of a target value within a sorted array, normally using pointers lo, mid, hi. Suitable from problems that contain sorted array, and requires O(logn) time and space complexity.

Example: Range searching
```javascript
/**
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.
Your algorithm's runtime complexity must be in the order of O(log n).
If the target is not found in the array, return [-1, -1].
Example 1:
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var searchRange = function(nums, target) {
    // if empty list
    if(nums.length === 0) return[-1,-1];
    var lo = 0;
    var hi = nums.length - 1;
    var mid;
    while(lo !== hi) {
        mid = Math.round((lo + hi) / 2);
        if(hi - lo === 1) {
            switch(nums[lo] === target) {
                case true:
                    break;
                case false:
                    lo = hi;
                    break;
            }
            break;
        }
        if(nums[mid] >= target) {
            hi = mid;
        }
        else if(nums[mid] < target) {
            lo = mid;
        }
    }
    // lo = mid on leftmost of the target values
    var start = lo;
    var end = lo;
    var range = new Array();
    if(nums[start] === target) {
        while(nums[end + 1] === target && end < nums.length) {
            end++;
        }
        range = [start,end];
    }
    else {
        range = [-1,-1];
    }
    return range;
};

```

### Dynamic Programming
The algorithm breaks the complicated problem down into sub-problems, and solve each sub-problem recursively. The steps are:
1. Construct array to contain optimal solution for each sub-problem
2. Recursively define the value of an optimal solution for each sub-problem
3. Fill up the array
4. Construct the optimal solution from the filled array

Dynamic programming can be implemented by either the top-down approach, that is, recursion, or the bottom-up approach called memorization. Bottom-up approach is particularly used to avoid any use of recursion to save the memory space, since what recursion does is to keep building up the call stack which is very memory-consuming.

An typical example for dynamic programming would be solving Fibonacci numbers. If want to compute Fib(22), we break it down into computing the values of Fib(20) and Fib(21). Thus we can define the solution recursively as Fib(n) = Fib(n-2) + Fib(n-1).

Suitable for problems: string problems

Example 1: The subarray with maximum sum
```javascript
/**
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
Example:
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if(nums.length === 1) {
        return nums[0]
    }
    var dp_array = new Array(nums.length);

    // initialize the array
    dp_array[0] = nums[0];
    var sol = nums[0];
    var i;
    for(i = 1; i < nums.length; i++) {
        dp_array[i] = Math.max(0, dp_array[i - 1]) + nums[i];
        sol = Math.max(dp_array[i], sol);
    }
    return sol;
};
```

Example 2: Staircase problem
```javascript
/**
You are climbing a stair case. It takes n steps to reach to the top.
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
Note: Given n will be a positive integer.
 * @param {number} n
 * @return {number}
 */
var climbStairs = function(n) {
    var climb_steps = new Array();
    climb_steps.push(1);
    climb_steps.push(1);
    var i;
    for(i = 2; i < n + 1; i++) {
        climb_steps[i] = climb_steps[i - 1] + climb_steps[i - 2];
    }
    return climb_steps[n];
};
```

Example 3: Find the unique paths
```javascript
/**
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
How many possible unique paths are there?
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function(m, n) {
    // corner cases
    if(m === 1 && n === 1) return 1;
    else if(m < 1 || n < 1) return 0;
    else if(m === 1 || n === 1) return 1;
    var paths = new Array();

    // initialization
    var i;
    var j;
    for(i = 0; i < n + 1; i++) {
        var ini_arr = new Array();
        for(j = 0; j < m + 1; j++) {
            ini_arr.push(0);
        }
        paths.push(ini_arr);
    }
    // (1,2) --> 1
    paths[1][2] = 1;
    // (2,1) --> 1
    paths[2][1] = 1;
    // (2,2) --> 2
    paths[2][2] = 2;
    console.log(paths)

    var row;
    var col;
    for(row = 1; row <= n; row++) {
        for(col = 1; col <= m; col++) {
            if(row == 1 && col === 1) continue;
            else if(row === 1 && col === 2) continue;
            else if(col === 1 && row === 2) continue;
            else if(row === 2 && col === 2) continue;
            else paths[row][col] = paths[row - 1][col] + paths[row][col - 1];
        }
    }
    return paths[n][m];
};
```
