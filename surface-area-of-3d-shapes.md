
## LeetCode每日一题（二十五）：三维形体的表面积

在 `N * N` 的网格上，我们放置一些 `1 * 1 * 1`  的立方体。

每个值 `v = grid[i][j]` 表示 `v` 个正方体叠放在对应单元格 `(i, j)` 上。

请你返回最终形体的表面积。


示例 1：

```
输入：[[2]]
输出：10
```

示例 2：

```
输入：[[1,2],[3,4]]
输出：34
```

示例 3：

```
输入：[[1,0],[0,2]]
输出：16
```

示例 4：

```
输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：32
```

示例 5：

```
输入：[[2,2,2],[2,1,2],[2,2,2]]
输出：46
```


提示：

* 1 <= N <= 50
* 0 <= grid[i][j] <= 50


## Solve

题目意思是这样的：

<div align="center">
    <img width="403px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/3d-shape.png"></img>
</div>

思路：作减法。

首先，我们需要知道有多少立方体，一个立方体有六个面。立方体个数 `x6` 则得到总面积。

每当一个立方体的一面被覆盖，其实是两个立方体的各自一面相互贴在一起。所以，只要知道有多少相互紧贴面的数量，再乘以2，即可得到所有被覆盖的面积。PS：本题底部面积也是表面积。

使用总面积 - 被覆盖的面积即可得到表面积。

```c++
class Solution {
public:
    int surfaceArea(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        int cubes = 0; // 记录立方体的数量
        int faces = 0; // 记录立方体接触面的数量
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                cubes += grid[i][j];
                if (grid[i][j] > 0) {
                    faces += grid[i][j] - 1; // 重叠的立方体接触面数量
                }
                if (i > 0) {
                    // 当前柱子与上边柱子的接触面数量
                    faces += min(grid[i-1][j], grid[i][j]);
                }
                if (j > 0) {
                    // 当前柱子与左边柱子的接触面数量
                    faces += min(grid[i][j-1], grid[i][j]);
                }
            }
        }
        return 6 * cubes - 2 * faces;
    }
};
```

* 时间复杂度：**O(n^2)**
* 空间复杂度：**O(1)**

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>