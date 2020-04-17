## LeetCode : Number of Islands

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input:
11110
11010
11000
00000

Output: 1
```

**Example 2:**

```
Input:
11000
11000
00100
00011

Output: 3
```


## Solve


#### 1.BFS

```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int rows = grid.size();
        if(rows == 0) return 0;
        // 注意：如果为空时，grid[0]不存，会出错
        int cols = grid[0].size();
        int dx[4] = {0, 0, -1, 1};
        int dy[4] = {-1, 1, 0, 0};
        int count = 0;
        queue<pair<int, int>> que;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == '1') {
                    count++;
                    que.push({i, j});   
                    while(!que.empty()) {
                        auto temp = que.front();
                        que.pop();
                        for(int i = 0; i < 4; i++) {
                            int x = temp.first + dx[i];
                            int y = temp.second + dy[i];
                            if(x < 0 || x >= rows || y < 0 || y >= cols || grid[x][y] != '1') 
                                continue;
                            else
                                grid[x][y] = '0';
                                que.push({x, y});
                        }
                    }
                }
            }
        }
        return count;
    }
};
```

* Time complexity : O(rows * cols)，其中 rows 和 cols 分别为行数和列数。
* Space complexity : O(min(rows, cols))


#### 2.DFS

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
        for(int i = 0; i < 4; i++) {
            int new_x = x + dx[i];
            int new_y = y + dy[i];
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

* Time complexity : O(rows * cols)，其中 rows 和 cols 分别为行数和列数。
* Space complexity : O(rows * cols)

