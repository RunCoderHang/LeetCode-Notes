## LeetCode每日一题（二十四）：按摩师

一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。

注意：本题相对原题稍作改动

示例 1：

```
输入： [1,2,3,1]
输出： 4
解释： 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。
```

示例 2：

```
输入： [2,7,9,3,1]
输出： 12
解释： 选择 1 号预约、 3 号预约和 5 号预约，总时长 = 2 + 9 + 1 = 12。
```

示例 3：

```
输入： [2,1,4,5,3,1,1,3]
输出： 12
解释： 选择 1 号预约、 3 号预约、 5 号预约和 8 号预约，总时长 = 2 + 4 + 3 + 3 = 12。
```


## Solve

#### 动态规划

根据题目，每当访问一个数组元素后，会越过其相邻的元素。如此间隔访问并相加。

当然，这里间隔的元素不一定只有一个，也可能是一个、两个、三个等等。但关键在于：如何间隔的数量大于等于一时，最后元素之和是最大值。

这里，我们使用动态规划方法。使用一个 `dp` 数组存储元素相加之和。假设访问数组的第 `i` 个元素时，第 `i-1` 与 第 `i` 个元素间隔一个 `i-2` 的元素，因此我们实现间隔相加时就是让 `i-2` 与 `i` 相加，如此循环。

当然，我们在求和的同时需要通过比较得出最大值。假如我们已经知道了前 `i-1` 和 `i-2` 个元素间隔相加之和：即 `dp[i-1]` 和 `dp[i-2]` 。

最后我们所得的递推方程为：

**`dp[i] = max(dp[i-1], dp[i-2] + nums[i])`**

```c++
class Solution {
public:
    int massage(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return 0;
        if(size == 1) return nums[0];
        int dp[size] = {0};
        dp[0] = nums[0];
        dp[1] = max(dp[0], nums[1]);
        for(int i = 2; i < size; i++) {
            dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
        }
        return dp[size-1];
    }
};
```

* 时间复杂度：**O(n)**
* 空间复杂度：**O(n)**

或者不需要数组

```c++
class Solution {
public:
    int massage(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return 0;
        if(size == 1) return nums[0];
        int temp = 0;
        int dp0 = nums[0];
        int dp1 = max(dp0, nums[1]);
        for(int i = 2; i < size; i++) {
            temp = max(dp1, dp0 + num[i]);
            dp0 = dp1;
            dp1 = temp;
        }
        return dp1;
    }
};
```

* 时间复杂度：**O(n)**
* 空间复杂度：**O(1)**


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>