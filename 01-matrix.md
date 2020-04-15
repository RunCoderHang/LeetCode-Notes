## LeetCode每日一题：01-矩阵

给定一个由 0 和 1 组成的矩阵，找出每个元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

**示例 1:**

输入:

```
0 0 0
0 1 0
0 0 0
```

输出:

```
0 0 0
0 1 0
0 0 0
```

**示例 2:**

输入:

```
0 0 0
0 1 0
1 1 1
```

输出:

```
0 0 0
0 1 0
1 2 1
```

注意:

1. 给定矩阵的元素个数不超过 10000。
2. 给定矩阵中至少有一个元素是 0。
3. 矩阵中的元素只在四个方向上相邻: 上、下、左、右。



## Solve

#### BFS

创建 `visited` ，记录是否访问的布尔类型。

遍历记录所有的0的位置且标记已访问，以及入队。根据四个方向进行广搜，若未访问则 `+1`。

```c++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int rows = matrix.size();
        int cols = matrix[0].size();
        queue<pair<int, int>> que;
        vector<vector<int>> res;
        vector<vector<bool>> visited(rows, vector<bool>(cols, false));
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(matrix[i][j] == 0) {
                    que.push({i, j});
                    visited[i][j] = true;
                }
            }
        }
        int dx[4] = {0, 0, 1, -1};
        int dy[4] = {1, -1, 0, 0};
        while(!que.empty()) {
            auto temp = que.front();
            que.pop();
            for(int i = 0; i < 4; i++) {
                int x = temp.first + dx[i];
                int y = temp.second + dy[i];
                if(x >= rows || x < 0 || y >= cols || y < 0 || visited[x][y])
                    continue;
                res[x][y] = res[temp.first][temp.second] + 1;
                visited[x][y] = true;
                que.push({x, y});
            }
        }
        return res;
    }
};
```

* 时间复杂度：O(rows * cols)
* 空间复杂度：O(rows * cols)

#### DP

状态转移方程：

`dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + 1` ，位置 `[i][j]` 的元素为 1

`dp[i][j] = 0` ，位置 `[i][j]` 的元素为 0

```c++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int rows = matrix.size();
        int cols = matrix[0].size();
        vector<vector<int>> dp(rows, vector(cols, INT_MAX / 2));
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(matrix[i][j] == 0)
                    dp[i][j] = 0;
            }
        }
        // 状态转移
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                // 水平向左
                if(i - 1 >= 0) dp[i][j] = min(dp[i][j], dp[i-1][j]+1);
                // 竖直向上
                if(j - 1 >= 0) dp[i][j] = min(dp[i][j], dp[i][j-1]+1); 
            }
        }
        for(int i = rows - 1; i >= 0; i--) {
            for(int j = cols - 1; j >= 0; j--) {
                // 水平向右
                if(i + 1 < rows) dp[i][j] = min(dp[i][j], dp[i+1][j]+1);
                // 数竖直向下
                if(j + 1 < cols) dp[i][j] = min(dp[i][j], dp[i][j+1]+1); 
            }
        }
        return dp;
    }
};
```


* 时间复杂度：O(rows * cols)
* 空间复杂度：O(1)

