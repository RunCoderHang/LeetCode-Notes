## LeetCode每日一题：最大子序和

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例:**

```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```


**进阶:**

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。


## 解题

#### 1. 暴力解法 (Accept)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return 0;
        if(size == 1) return nums[0];
        int maxSum = INT_MIN;
        for(int i = 0; i < size; i++) {
            int curSum = nums[i];
            maxSum = max(maxSum, curSum);
            for(int j = i+1; j < size; j++) {
                curSum += nums[j];
                maxSum = max(maxSum, curSum);
            }
        }
        return maxSum;
    }
};
```

* 时间复杂度： O(n^2)
* 空间复杂度： O(1)


#### 2. 一次遍历

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {  
        int maxSum = nums[0];
        int curSum = 0;
        for(int i = 0; i < nums.size(); i++) {
            curSum += nums[i];
            maxSum = max(curSum, maxSum);
            if(curSum < 0) curSum = 0;
        }
        return maxSum;
    }
};
```

* 时间复杂度： O(n)
* 空间复杂度： O(1)
