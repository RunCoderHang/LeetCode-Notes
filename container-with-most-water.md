## LeeCode每日一题：盛最多水的容器

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

<div align="center">
    <img width="650px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/container-with-most-water.jpg"></img>
</div>

图中垂直线代表输入数组 `[1,8,6,2,5,4,8,3,7]` 。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。


示例：

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```


## Solve

#### 双指针

定义两个指针： `left` 、 `right` 分别指向数组的两端，向中间检索。

遍历时需要判断两个指针指向的高度的大小关系，并且记录水的容量。公式为： `min(height[left], height[right]) * size` 。依次寻找水的最大容量。

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int size = height.size();
        int left = 0, right = size - 1;
        int water = 0;
        while(left < right) {
            int temp = min(height[left], height[right]) * size;
            cout << temp << endl;
            water = max(water, min(height[left], height[right]) * (size-1));
            if(height[left] > height[right])
                right--;
            else
                left++;
            size--;
        }
        return water;
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(1)


