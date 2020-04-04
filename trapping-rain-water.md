## LeetCode每日一题：接雨水

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

上面是由数组 `[0,1,0,2,1,0,1,3,2,1,2,1]` 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

<div align="center">
    <img width="300px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/rainwatertrap.png"></img>
</div>


示例:

```
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6
```


## Solve

#### 默哀

<div align="center">
    <img width="661px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/silence-tribute.jpg"></img>
</div>

**英烈为国家舍生忘死，逝者同胞亦盼山河无恙。**

<br>

#### 1.暴力解法 (超时)

遍历一遍数组中的每个元素，也就是每个柱子的高度。

在遍历的时候，设置 `leftMax` 与 `rightMax` 分别代表该柱子的左右最大柱子的高度。

若要接住水，一定是左右柱子比中间的柱子要高，这样形成一个凹槽才能盛放水。因此，我们一边遍历每一个柱子，一边遍历该柱子左右元素找到左右最高的柱子。

假如左右最高柱子高度为： 2 、 3 ，中间没有柱子。接住水的高度是根据 **左右柱子高度的最小值** 计算。所得计算公式为：

$res += (min(leftMax, rightMax) - height[i])$

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int size = height.size();
        int res = 0;
        for(int i = 1; i < size - 1; i++) {
            int leftMax = 0, rightMax = 0;
            for(int j = i; j >= 0; j--) {
                leftMax = max(leftMax, height[j]);
            }

            for(int k = i; k < size; k++) {
                rightMax = max(rightMax, height[k]);
            }

            res += (min(leftMax, rightMax) - height[i]);
        }
        return res;
    }
};
```

* 时间复杂度：$O(n^2)$
* 空间复杂度：$O(1)$


#### 2.优化：双指针

当所有凹槽盛满水时的状态，我们不难发现这是一个驼峰形状：中间高，两边高度依次降低。并且那些盛满水的凹槽中，水的高度就是两边最高高度减去柱子高度。

所以根据上面方法，我们其实不需要外层的循环。仅需要两个指针指向数组的两端，向中间遍历。

遍历过程中记录两边的最高高度，并利用最高高度减去柱子高度。所得水量依次相加。

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int size = height.size();
        if(size == 0) return 0;
        int res = 0;
        int maxPosition = max_element(height.begin(), height.end()) - height.begin();
        int leftMax = height[0];
        for(int i = 1; i < maxPosition; i++) {
            leftMax = max(leftMax, height[i]);
            res += (leftMax - height[i]);
        }
        int rightMax = height[size-1];
        for(int i = size - 2; i > maxPosition; i--) {
            rightMax = max(rightMax, height[i]);
            res += (rightMax - height[i]);
        }
        
        return res;
    }
};
```

* 时间复杂度：$O(n)$
* 空间复杂度：$O(1)$

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
