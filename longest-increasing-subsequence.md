## LeetCode每日一题（十四）：最长上升子序列

给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

说明:

* 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
* 你算法的时间复杂度应该为 O(n2) 。


## Solve

#### 1.动态规划

定义 `dp[i]` 为考虑前 i 个元素，以第 i 个数字结尾的最长上升子序列的长度。

从小到大计算 `dp[]` 数组的值，在计算 `dp[i]` 之前，我们已经计算出 `dp[0 ... i-1]` 的值，则状态转移方程为：

`dp[i] = max( dp[i] ) + 1` 其中 `0 <= j < i` 且 `num[j] < num[i]`

最后，整个数组的最长上升子序列即所有 `dp[i]` 中的最大值。

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n=(int)nums.size();
        if (n == 0) return 0;
        vector<int> dp(n, 0);
        for (int i = 0; i < n; ++i) {
            dp[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
        }
        // 求向量中的最大值
        return *max_element(dp.begin(), dp.end());
    }
};
```

* 时间复杂度：O(n^2)
* 空间复杂度：O(n)

#### 2.贪心 + 二分查找

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len = 1, n = (int)nums.size();
        if (n == 0) return 0;
        vector<int> d(n + 1, 0);
        d[len] = nums[0];
        for (int i = 1; i < n; ++i) {
            if (nums[i] > d[len]) d[++len] = nums[i];
            else{
                int l = 1, r = len, pos = 0; // 如果找不到说明所有的数都比 nums[i] 大，此时要更新 d[1]，所以这里将 pos 设为 0
                while (l <= r) {
                    int mid = (l + r) >> 1;
                    if (d[mid] < nums[i]) {
                        pos = mid;
                        l = mid + 1;
                    }
                    else r = mid - 1;
                }
                d[pos + 1] = nums[i];
            }
        }
        return len;
    }
};
```

* 时间复杂度：O(nlogn)
* 空间复杂度：O(n)

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://runcoderhang.github.io/thumbnails/wxgzh-hang.png">
</div>
