## LeetCode : Maxiaml Square

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

**Example:**

Input: 

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output: 4
```

## Solve

#### 1. 暴力解法 (Brute Force)

```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int rows = matrix.size(), cols = 0;
        if(rows > 0)
            cols = matrix[0].size();
        int maxsqlen = 0;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(matrix[i][j] == '1') {
                    int sqlen = 1;
                    bool flag = true;
                    while(sqlen + i < rows && sqlen + j < cols && flag) {
                        for(int k = j; k <= sqlen + j; k++) {
                            if(matrix[i+sqlen][k] == '0') {
                                flag = false;
                                break;
                            }
                        }
                        for(int k = i; k <= sqlen + i; k++) {
                            if(matrix[k][j+sqlen] == '0') {
                                flag = false;
                                break;
                            }
                        }
                        if(flag) sqlen++;
                    }
                    maxsqlen = max(maxsqlen, sqlen);
                }
            }
        }
        return maxsqlen * maxsqlen;
    }
};
```

* Time complexity : O((rows * cols)^2)
* Space complexity : O(1)

#### 2. 动态规划 (Dynamic Programming)

```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int rows = matrix.size(), cols = 0;
        if(rows > 0)
            cols = matrix[0].size();
        int maxsqlen = 0;
        vector<vector<int>> dp(rows+1, vector<int>(cols+1, 0));
        for(int i = 1; i <= rows; i++) {
            for(int j = 1; j <= cols; j++) {
                if(matrix[i-1][j-1] == '1') {
                    dp[i][j] = min(min(dp[i][j-1], dp[i-1][j]), dp[i-1][j-1]) + 1;
                    maxsqlen = max(maxsqlen, dp[i][j]);
                }
            }
        }
        return maxsqlen * maxsqlen;
    }
};
```

* Time complexity : O(rows * cols)
* Space complexity : O(rows * cols)


