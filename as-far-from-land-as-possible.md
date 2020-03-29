## LeetCode每日一题（二十九）：地图分析

你现在手里有一份大小为 N x N 的『地图』（网格） `grid`，上面的每个『区域』（单元格）都用 `0` 和 `1` 标记好了。其中 `0` 代表海洋，`1` 代表陆地，你知道距离陆地区域最远的海洋区域是是哪一个吗？请返回该海洋区域到离它最近的陆地区域的距离。

我们这里说的距离是『曼哈顿距离』（ Manhattan Distance）：`(x0, y0)` 和 `(x1, y1)` 这两个区域之间的距离是 `|x0 - x1| + |y0 - y1|` 。

如果我们的地图上只有陆地或者海洋，请返回 -1。

 

示例 1：

| 1 | 0 | 1 |
| 0 | 0 | 0 |
| 1 | 0 | 1 |

```
输入：[[1,0,1],[0,0,0],[1,0,1]]
输出：2

解释： 
海洋区域 (1, 1) 和所有陆地区域之间的距离都达到最大，最大距离为 2。
```

示例 2：

| 1 | 0 | 0 |
| 0 | 0 | 0 |
| 0 | 0 | 0 |

```
输入：[[1,0,0],[0,0,0],[0,0,0]]
输出：4

解释： 
海洋区域 (2, 2) 和所有陆地区域之间的距离都达到最大，最大距离为 4。
```

提示：

* 1 <= grid.length == grid[0].length <= 100
* grid[i][j] 不是 0 就是 1


## Solve

#### BFS

类似于[腐烂的橘子](https://mp.weixin.qq.com/s/4jSq8gZwtcrtpxzRNWpvig)的题目。

**题目解析：** 找寻距离陆地区域最远的海洋区域，二维地图模拟出来就是从陆地出发，以上、下、左、右四个方向遍历找寻最远的海洋区域。每四个方向遍历一次，距离增加一次。

然而，给出的数据中不只有一个陆地区域，也就是多个源点。

使用广度优先搜索时，就需要队列对元素进行存储。首先，我们需要将陆地区域存入队列，而这些陆地区域就是第0层。这也就是为什么距离的初始值为 `-1` 。

随后，我们根据该层的元素数量一次性遍历，寻找周围的海洋区域。找寻海洋区域同时改写为 `2` 变成非海洋区域，并且入队。

循环上面的步骤时距离需要循环 `+1`，直到遍历完所有的海洋区域。最后得到的即是最远的海洋区域距离。，因为广度优先搜索的过程就是由近到远的过程。

<div align="center">
    <img width="410px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/BFS.gif"></img>
</div>

如图所示，当 `distance = 4` 时，改成元素出队，下一层元素入队。随后，`distance = 5` 时本层元素出队，没有元素入队，退出循环。

```c++
class Solution {
public:

    int maxDistance(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        queue<int> queue_x;
        queue<int> queue_y;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == 1) { // 所有的陆地入队
                    queue_x.push(i);
                    queue_y.push(j);
                }
            }
        }
        // 上、下、左、右四个方向
        int dx[4] = {-1, 1, 0, 0};
        int dy[4] = {0, 0, -1, 1};

        // 全是陆地或全是海洋的话，返回 -1
        if(queue_x.empty() || queue_y.empty() || queue_x.size() == rows*cols || queue_y.size() == rows*cols) 
            return -1;

        int distance = -1;  // 记录最远距离
        while(!queue_x.empty()) {   // 从陆地出发，广度逐层遍历。
            distance++;
            int size = queue_x.size();  // 该层的元素的数量
            while(size--) { // 遍历完该层元素
                int cur_x = queue_x.front(), cur_y = queue_y.front();
                queue_x.pop();
                queue_y.pop();
                for(int i = 0; i < 4; i++) {
                    int new_x = cur_x + dx[i];
                    int new_y = cur_y + dy[i];
                    
                    if (new_x < 0 || new_x >= rows || new_y < 0 || new_y >= cols || grid[new_x][new_y] != 0) {
                       continue;
                    }
                    grid[new_x][new_y] = 2; // 改写为非海洋区域
                    queue_x.push(new_x);
                    queue_y.push(new_y);
                }
            }
        }
        return distance;
    }

};
```


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>