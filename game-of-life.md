## LeetCode每日一题：生命游戏

根据 [百度百科](https://baike.baidu.com/item/%E7%94%9F%E5%91%BD%E6%B8%B8%E6%88%8F/2926434?fr=aladdin) ，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。

给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：1 即为活细胞（live），或 0 即为死细胞（dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

* 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
* 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
* 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
* 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

根据当前状态，写一个函数来计算面板上所有细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

 

**示例：**

```
输入： 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
输出：
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```


**进阶：**

* 你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
* 本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？


## Solve

本题没有特殊的解法，仅需要判断二维数组每个元素的周围的活细胞数量是否符合存活规则。

不过本题中需要八个位置。那不就是在九宫格中间，外面的的八个位置。值得注意的一点是：死亡以及复活是一次过程中同时发生的。这就把我难住了。要思考：**怎么才能在一个细胞死亡或复活后，而不影响其他细胞的生命。**

我参考了大佬们的做法：**位运算**。利用 `0`、 `1`、 `2`、 `3` 的二进制进行位运算。

除了 `0` 以外，其他数字的二进制与 `1` 进行 `&` 与运算，最后得出的结果都是 `1` 。利用与运算来计数活细胞的数量。

```c++
if(board[x][y] & 1 == 1) cnt++; // 活细胞计数
```

为什么要用与运算？

因为，我们需要上面的几个数字来代表细胞状态。当死亡和存活用 `1` `2` `3` 来代替发生的变化，这样我们在遍历每个细胞时，**都不影响周围活细胞的数量**。

接下来就是如何区别细胞变化的状态。

```
当细胞为活细胞时 (== 1) {

    如果周围活细胞数少于两个或者超过三个 (cnt < 2 || cnt > 3)
        活细胞死亡 1 | 0 = 1

    如果周围有两个或三个活细胞
        活细胞仍然存活 1 | 2 = 3
}

当细胞为死细胞时 (== 0) {

    如果死细胞周围正好有三个活细胞
        死细胞复活 0 | 2 = 2

    否则
        没有复活 0 | 0 = 0
}
```

然后得出结果，然而这不是最终的结果。最后，我们需要移位运算 `>>`

```
0 >> 1 = 0
1 >> 1 = 0
2 >> 1 = 1
3 >> 1 = 1

|0|1|0|         |0|1|0|          |0|0|0|
|0|0|1|   -->   |2|0|3|   -->    |1|0|1|     
|1|1|1|         |1|3|3|          |0|1|1|
|0|0|0|         |0|2|0|          |0|1|0|
```

```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        if(board.size() == 0) return;
        int rows = board.size();
        int cols = board[0].size();

        //          上 下 左 右 左上 左下 右上 右下
        int dx[8] = {-1, 1, 0, 0, -1,  1, -1, 1};
        int dy[8] = { 0, 0,-1, 1, -1, -1,  1, 1};

        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                int cnt = 0; // 周围活细胞的数量
                for(int k = 0; k < 8; k++) {
                    int x = i + dx[k];
                    int y = j + dy[k];
                    if(x < 0 || y < 0 || x >= rows || y >= cols) {
                        continue;
                    }
                    if(board[x][y] & 1 == 1) cnt++; 
                }
                if(board[i][j] == 1) { // 活细胞情况
                    if(cnt < 2 || cnt > 3) {  // 如果活细胞周围活细胞数少于两个或超过三个
                        board[i][j] |= 0; // 活细胞死亡 1 | 0 = 1
                    } else {    // 如果活细胞周围有两个或三个活细胞
                        board[i][j] |= 2; // 活细胞仍然存活 1 | 2 = 3
                    }
                } else {
                    if(cnt == 3) {  // 如果死细胞周围正好有三个活细胞
                        board[i][j] |= 2; // 死细胞复活 0 | 2 = 2
                    } else {
                        board[i][j] |= 0; // 否则，没有复活 0 | 0 = 0
                    }
                }
            }
        }
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                board[i][j] >>= 1;  //右移一位
            }
        }
    }
};
```

* 时间复杂度：**O(nm)**
* 空间复杂度：**O(1)**

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>

