## LeetCode每日一题：岛屿数量

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1:

```
输入:
11110
11010
11000
00000

输出: 1
```

示例 2:

```
输入:
11000
11000
00100
00011

输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```



## Solve

**题意：** 一个岛屿指的是所有 `'1'` 在四个方向连接起来的岛屿，因此要求得图中所有的岛屿数量。

**题解：** 遍历寻找岛屿，登陆后，岛屿数量 `+1` ，接着向四周寻找是否有群岛，如果有就一个一个炸沉（赋值为 `'0'`），没错就是这么暴力 :)

#### 1. BFS

```c++
class Solution {
    int count = 0;
    int dx[4] = {0, 0, -1, 1};
    int dy[4] = {-1, 1, 0, 0};
public:
    int numIslands(vector<vector<char>>& grid) {
        int rows = grid.size();
        if(rows == 0) return 0;
        int cols = grid[0].size();
        queue<pair<int, int>> que;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == '1') {
                    count++;
                    que.push({i, j});
                }
                while(!que.empty()) {
                    auto temp = que.front();
                    que.pop();
                    for(int dire = 0; dire < 4; dire++) {
                        int x = temp.first + dx[dire];
                        int y = temp.second + dy[dire];
                        if(x < 0 || x >= rows || y < 0 || y >= cols || grid[x][y] != '1')
                            continue;
                        else
                            grid[x][y] = '0';
                            que.push({x, y});
                    }
                }
            }
        }
        return count;
    }
};
```

* 时间复杂度：O(rows * cols)
* 空间复杂度：O(min(rows, cols))

#### 2. DFS

```c++
class Solution {
    int count = 0;
    int dx[4] = {0, 0, -1, 1};
    int dy[4] = {-1, 1, 0, 0};
public:
    void dfs(vector<vector<char>>& grid, int x, int y) {
        if(x < 0 || x >= grid.size() || y < 0 || y >= grid[0].size() || grid[x][y] != '1')
            return;
        grid[x][y] = '0';
        for(int dire = 0; dire < 4; dire++) {
            int new_x = x + dx[dire];
            int new_y = y + dy[dire];
            dfs(grid, new_x, new_y);
        }
    }
    int numIslands(vector<vector<char>>& grid) {
        int rows = grid.size();
        if(rows == 0) return 0;
        int cols = grid[0].size();
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == '1')
                    count++;
                    dfs(grid, i, j);
            }
        }
        return count;
    }
};
```

* 时间复杂度：O(rows * cols)
* 空间复杂度：O(rows * cols)


