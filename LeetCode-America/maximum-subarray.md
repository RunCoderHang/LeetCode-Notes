## LeetCode : Maximum Subarray

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
Follow up:

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.


## Solve

#### 双指针

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        if(nums.size() == 1) return nums[0];
        int maxSum = INT_MIN;
        for(int i = 0; i < nums.size(); i++) {
            int sum = nums[i];
            maxSum = max(maxSum, sum);
            for(int j = i + 1; j < nums.size(); j++) {
                sum += nums[j];
                maxSum = max(maxSum, sum);
            }
        }
        return maxSum;
    }
};
```

* Time complexity : **O(n^2)**
* Space complexity : **O(1)**


**优化：一次遍历**

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0];
        int sum = 0;
        for(int i = 0; i < nums.size(); i++) {
            sum += nums[i];
            maxSum = max(maxSum, sum);
            if(sum < 0) sum = 0;
        }
        return maxSum;
    }
};
```

* Time complexity : **O(n)**
* Space complexity : **O(1)**

