## LeetCode每日一题（十八）：矩形重叠

矩形以列表 `[x1, y1, x2, y2]` 的形式表示，其中 `(x1, y1)` 为左下角的坐标，`(x2, y2)` 是右上角的坐标。

如果相交的面积为正，则称两矩形重叠。需要明确的是，只在角或边接触的两个矩形不构成重叠。

给出两个矩形，判断它们是否重叠并返回结果。

示例 1：

```
输入：rec1 = [0,0,2,2], rec2 = [1,1,3,3]
输出：true
```

示例 2：

```
输入：rec1 = [0,0,1,1], rec2 = [1,0,2,1]
输出：false
```

提示：

1. 两个矩形 rec1 和 rec2 都以含有四个整数的列表的形式给出。
2. 矩形中的所有坐标都处于 -10^9 和 10^9 之间。
3. x 轴默认指向右，y 轴默认指向上。
4. 你可以仅考虑矩形是正放的情况。

## Solve

根据题意，判断给出的两个矩形的 **左下角** 与 **右下角** 坐标的大小关系。

例如，如果 矩形1 的左下角与 矩形2 的右上角没有重叠，则两个矩形一定不会重叠。

同理，如果 矩形1 的右上角与 矩形2 的左下角没有重叠，则两个矩形一定不会重叠。

因此，只要反向思考不重叠的情况，结合画图即可解出。

```c++
class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        if(rec1[1] >= rec2[3] || rec1[3] <= rec2[1] || rec1[2] <= rec2[0] || rec1[0] >= rec2[2])
            return false;
        return true;
    }
};
```

```c
bool isRectangleOverlap(int* rec1, int rec1Size, int* rec2, int rec2Size){
    return !(rec1[2] <= rec2[0] || rec2[2] <= rec1[0] || rec1[3] <= rec2[1] || rec2[3] <= rec1[1]);
}
```

* 时间复杂度：O(1)
* 空间复杂度：O(1)

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
