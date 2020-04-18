## LeetCode : Minimum Path Sum

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example:**

```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```


## Solve

典型的DP问题，我竟然还在想怎么用BFS。

#### DP

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int rows = grid.size();
        if(rows == 0) return 0;
        int cols = grid[0].size();
        vector<vector<int>> dp(rows, vector<int>(cols, 0));
        dp[0][0] = grid[0][0];
        for(int i = 1; i < rows; i++)
            dp[i][0] = dp[i-1][0] + grid[i][0];
        for(int i = 1; i < cols; i++)
            dp[0][i] = dp[0][i-1] + grid[0][i];
        for(int i = 1; i < rows; i++) {
            for(int j = 1; j < cols; j++) {
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }
        return dp[rows-1][cols-1];
    }
};
```

* Time conplexity: O(n)
* Space complexity: O(n)


**优化空间**

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int rows = grid.size();
        if(rows == 0) return 0;
        int cols = grid[0].size();
        
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols; j++){
                if(i > 0 and j > 0) 
                    grid[i][j] += min(grid[i][j-1], grid[i-1][j]);
                else if(i == 0 and j > 0) 
                    grid[i][j] += grid[i][j-1];
                else if(i > 0 and j == 0) 
                    grid[i][j] += grid[i-1][j];
            }
        }
        
        return grid[rows-1][cols-1];
    }
};
```

* Time conplexity: O(n)
* Space complexity: O(1)

